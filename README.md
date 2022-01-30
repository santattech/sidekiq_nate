# How to handle Sidekiq in Practice step wise

1. gem install skp
2. If you purchased the Workshop yourself, you will receive a Slack channel invitation shortly. If you are attending the Workshop as part of a group and your license key was provided to you, you need to register your key to get an invite:

$ skp key register [YOUR_EMAIL_ADDRESS]
In my case, it is my office email address.
Please note you can only register your key once.

3. skp start
4. Few Important commands
Here are some important commands for you to know:

$ skp next     | Proceed to the next part of the workshop.
$ skp complete | Mark current lesson as complete.
$ skp list     | List all workshop lessons. Shows progress.
$ skp download | Download all lessons. Useful for offline access.
$ skp show     | Show any particular workshop lesson.
$ skp current  | Opens the current lesson.
Generally, you'll just be doing a lot of $ skp next! It's the same thing as $ skp complete && skp show.

# Determine sidekiq statistics
```
    workers = Sidekiq::CLI.instance&.launcher&.manager&.workers
    num_processors_busy = workers&.count(&:job) || 0
    num_processors_total = workers&.size || 0
    util_percent = num_processors_busy.to_f / num_processors_total * 100

    default_queue_latency_in_seconds = Sidekiq::Stats.new.default_queue_latency.round # Maybe in Sidekiq::API?

    retry_queue_latency_in_seconds = Sidekiq::Queue.new("retry").latency # Maybe in Sidekiq::API?

    Sidekiq.logger.warn(Sidekiq::CLI.r + "*" * 20 + Sidekiq::CLI.reset)
    Sidekiq.logger.warn(
      "Utilization (instantaneous): #{util_percent}% | " +
      "Default latency: #{default_queue_latency_in_seconds} | " +
      "Retries latency: #{retry_queue_latency_in_seconds}"
    )
    Sidekiq.logger.warn(Sidekiq::CLI.r + "*" * 20 + Sidekiq::CLI.reset)
    


```