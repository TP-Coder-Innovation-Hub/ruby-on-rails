# Background Jobs `[Senior]`

## Why Background Jobs?

Some work takes too long to run during a web request. If sending a welcome email takes 2 seconds, the user waits 2 seconds staring at a spinner. If generating a report takes 30 seconds, the request times out.

Background jobs move slow work out of the request cycle. The user gets an immediate response. The work happens later.

Common use cases:

- Sending emails
- Processing images or files
- Generating reports
- Syncing data with external APIs
- Scheduled tasks (daily cleanup, weekly digests)

## Solid Queue (Rails 8 Default)

Rails 8 ships with Solid Queue, a database-backed job queue. No Redis. No external dependencies. It uses your existing PostgreSQL database.

### Configuration

```ruby
# config/application.rb
config.active_job.queue_adapter = :solid_queue
```

```ruby
# config/queue.yml
production:
  dispatchers: 1
  workers: 3
```

### Creating a Job

```bash
bin/rails generate job send_welcome_email
```

```ruby
# app/jobs/send_welcome_email_job.rb
class SendWelcomeEmailJob < ApplicationJob
  queue_as :mailers

  def perform(user_id)
    user = User.find(user_id)
    UserMailer.welcome(user).deliver_now
  end
end
```

### Enqueueing a Job

```ruby
# Enqueue and return immediately
SendWelcomeEmailJob.perform_later(user.id)

# Enqueue with a delay
SendWelcomeEmailJob.set(wait: 5.minutes).perform_later(user.id)

# Enqueue for a specific time
SendWelcomeEmailJob.set(wait_until: Date.tomorrow.noon).perform_later(user.id)

# Enqueue with priority
SendWelcomeEmailJob.set(priority: 10).perform_later(user.id)
```

## Sidekiq

For high-throughput applications (10,000+ jobs per minute), Sidekiq is the established solution. It uses Redis for job storage.

### When to Choose Sidekiq over Solid Queue

- You process more than 10,000 jobs per minute
- You need Redis for other purposes (caching, real-time features)
- You need the Sidekiq Pro or Enterprise features (reliable fetch, batching)
- You have an existing Sidekiq infrastructure

```ruby
# config/application.rb
config.active_job.queue_adapter = :sidekiq
```

```ruby
# Gemfile
gem "sidekiq"
```

Both Solid Queue and Sidekiq use the same `ActiveJob` interface. Switching between them requires only a configuration change.

## Job Best Practices

### Keep Jobs Small

```ruby
# Bad: one massive job
class ProcessOrderJob < ApplicationJob
  def perform(order_id)
    order = Order.find(order_id)
    charge_customer(order)
    send_confirmation(order)
    update_inventory(order)
    notify_warehouse(order)
    generate_invoice(order)
  end
end

# Good: separate jobs for separate concerns
class ChargeCustomerJob < ApplicationJob
  def perform(order_id)
    order = Order.find(order_id)
    PaymentService.charge(order)
  end
end

class SendConfirmationJob < ApplicationJob
  def perform(order_id)
    order = Order.find(order_id)
    OrderMailer.confirmation(order).deliver_now
  end
end
```

Small jobs are easier to retry, debug, and monitor.

### Use Job Arguments Wisely

Pass IDs, not full objects. Jobs are serialized to the database. Objects can become stale.

```ruby
# Bad
SendEmailJob.perform_later(user)

# Good
SendEmailJob.perform_later(user.id)
```

### Handle Failures

```ruby
class ImportDataJob < ApplicationJob
  queue_as :imports

  retry_on ExternalApiError, wait: 30.seconds, attempts: 3
  discard_on InvalidDataError

  def perform(import_id)
    import = Import.find(import_id)
    ExternalApi.fetch_and_process(import)
  end
end
```

- `retry_on` -- Retry on transient errors (network timeouts, rate limits)
- `discard_on` -- Give up on permanent errors (bad data, missing records)
- `discarded` and `failed` callbacks for monitoring and alerting

### Idempotency

A job should produce the same result if run multiple times. Jobs can be retried. If a job is not idempotent, a retry causes duplicate work.

```ruby
# Bad: sends duplicate emails on retry
def perform(user_id)
  user = User.find(user_id)
  UserMailer.welcome(user).deliver_now
end

# Good: checks if already sent
def perform(user_id)
  user = User.find(user_id)
  return if user.welcome_email_sent?
  UserMailer.welcome(user).deliver_now
  user.update!(welcome_email_sent_at: Time.current)
end
```

## Scheduled Jobs

For recurring tasks, use `SolidQueue::RecurringJob` (Rails 8) or `sidekiq-cron`:

```yaml
# config/recurring.yml
production:
  daily_cleanup:
    class: CleanupJob
    schedule: "0 2 * * *"  # 2 AM daily
  weekly_digest:
    class: WeeklyDigestJob
    schedule: "0 9 * * 1"  # 9 AM every Monday
```

Move to `04-deployment.md` to ship your application to production.
