# Monitoring-Windows-Server-With-prometheus-and-Grafana
Monitoring Windows Server With prometheus and Grafana

# Monitoring Windows OS with Prometheus
Prometheus Community exporter Windows Exporter can monitor desktops/servers with the Windows operating systems installed using Prometheus.
## Pre-requisites
To monitor a Windows computer with Prometheus, you must have Prometheus installed on your computer. You also need to have the Prometheus Windows Exporter installed on your Windows computer.
## Downloading Windows Exporter
To download Prometheus Windows Exporter, navigate to the [GitHub page of Prometheus Community’s Windows Exporter](https://github.com/prometheus-community/windows_exporter) from your favorite web browser.

Scroll down to the Assets section. You have to download the correct version of the Windows Exporter installer for your computer from here.

If you’re running a 32-bit Windows operating system version, click on the windows_exporter-*-386.msi link.

If you’re running a 64-bit Windows operating system version, click on the windows_exporter-*-amd64.msi link.

Windows Exporter installer should be downloaded.

## Installing Windows Exporter
The Downloads\ folder should be in the path C:\Users\<USERNAME>\Downloads. Replace <USERNAME> with the username of your Windows operating system.

In my case, the Downloads\ folder path is C:\Users\Administrator\Downloads
Now, search for the Command Prompt app, right-click (RMB) on it, and click on Run as administrator as marked in the screenshot below.
Run the following command to navigate to the Downloads/ folder of your computer.
```
cmd> cd "C:\Users\<USERNAME>\Downloads"
```
NOTE: Make sure to replace <USERNAME> with the username of your Windows computer.

To install Windows Exporter and enable only the default collectors, run the following command:
```
cmd> msiexec /i .\windows_exporter-0.16.0-amd64.msi
```
To install Windows Exporter and enable only the default collectors as well as 
the non-default memory collector, run the following command:
```
cmd> msiexec /i windows_exporter-0.16.0-amd64.msi ENABLED_COLLECTORS="[defaults],memory"
```
Windows Exporter should be running on port 9182 of your Windows computer.

To verify whether Windows Exporter is working, open a web browser and visit http://localhost:9182/metrics. If you see the following output, then Windows Exporter is working.
## Adding Windows Exporter to Prometheus
Once the Windows Exporter is installed on your Windows computer, you should be able to add it to Prometheus.

Now, open the Prometheus configuration file /opt/prometheus/prometheus.yml with the nano text editor as follows:
```
$ sudo nano /opt/prometheus/prometheus.yml
```
Type in the following lines in the scrape_configs section of the /opt/prometheus/prometheus.yml file. Make sure to replace the IP address with the IP address of your Windows computer.
```
  - job_name: 'win10'
    static_configs:
    - targets: ['192.168.3.130:9182']
```

For the changes to take effect, restart the prometheus systemd service as follows:
```
$ sudo systemctl restart prometheus.service
```
