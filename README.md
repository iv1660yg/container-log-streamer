# container-log-streamer
A straightforward guide and tools to forward application logs to stdout in Docker containers, making logs accessible through . Ideal for monitoring and debugging containerized applications effectively.

## 📋 Steps to Enable Log Forwarding

### 1. Enable Log Files
Ensure that the application generates log files and locate their default log directory. Refer to the application's documentation if necessary to enable logging or specify the log file path.


### 2. Map Log Directory to Host
Add a volume mapping in the `docker-compose.yml` file to make the log directory accessible on the host machine:

```yaml
volumes:
  - /path/on/host:/path/to/container/logs
````

This maps the container's log directory () to a directory on the host ().


### 3. Tail Log Files
Use the ```tail -f``` on the host machine to view logs in real-time:

```bash
tail -f /path/on/host/logfile.log
```
This will stream new log entries as they are written.

### 4. Automate Log Forwarding to stdout
Forward log files directly to ```stdout``` by using a simple log forwarding script. Follow these steps:

a. Create a Log Forwarding Script
Create a script named ```log_forward.sh``` with the following content:

```bash
#!/bin/sh
tail -f /path/to/container/logs/logfile.log
```

b. Make the Script Executable
Grant execution permissions to the script:
```bash
chmod +x /path/to/log_forward.sh
```

c. Update ```docker-compose.yaml``` to Use the Script
Modify your ```docker-compose.yaml``` to use the script as the container's start-up command:

```yaml
command: /path/to/container/log_forward.sh
```

### 5. View Logs via Docker
a. Once the container is running, view the forwarded logs using:
```bash
docker logs -f <container_name>
```
b. Use Real-Time Logging & Monitoring Apps
For a more visual and user-friendly approach, you can use apps like [Dozzle](https://github.com/amir20/dozzle), which provide real-time web-based log monitoring. 

### 📝 Notes
* Replace ```/path/to/container/logs```and ```/path/on/host``` with the appropriate paths for your application.
* Ensure the application is actively writing to the log file specified
* For more complex use cases, consider integrating log aggregation tools like Fluentd or Logstash.

#### 💡 Example Docker Compose File
Here’s an example configuration for reference:
```yaml
services:
  myapp:
    image: myapp:latest
    container_name: myapp
    volumes:
      - /path/on/host:/path/to/container/logs
    command: /path/to/container/log_forward.sh
    restart: unless-stopped
```
