depremkontrol
This is a simple .NET Core 3.0 BackgroundService application. New BackgroundService API provides implementation ways to create long-running background workers, asynchronous tasks, schedule tasks in operating systam. In *Unix kind OS, this kind of .NET Core application also works as systemd service with unit files.

This application can run on Rasbian OS, Raspberry Pi device.

Background Task
This application fetches earthquake data from Boğaziçi University Kandilli Observatory And Earthquake Research Institute's web page. The background job parses HTML content with regular expression due to no reliable API exists.

New BackgroundServicein .NET Core 3.0 provides steady flow for background jobs. With IHostApplicationLifetime registering ApplicationStartted, ApplicationStarting and ApplicationStopped events are simple so control over job is more solid.

The main structure of long-running operation is written in ExecuteAsync(CancellationToken stoppingToken) method. Please check Worker.cs

        protected override async Task ExecuteAsync(CancellationToken stoppingToken)
        {
            try
            {
                while (!stoppingToken.IsCancellationRequested)
                {
                    .............
                    ..........
                    .......
                    .....
                    ....
                    //Wait a little bit for next check
                    await Task.Delay(_settings.Value.Period, stoppingToken);
                }
            }
            catch (Exception ex)
            {

            }
        }
Unit file
Check earthquake.service file for service description for *Unix systems.

After creating the file, be sure to copy it to /etc/systemd/system path.

Example:

cp /home/pi/projects/earthquake-checker/earthquake.service /etc/systemd/system/earthquake.service

Some service operations;

systemctl daemon-reload         # make systemd reload the unit files to reflect changes
systemctl start earthquake      # start the service
systemctl stop earthquake       # start the service
systemctl enable earthquake     # install the service so it is started automatically
More fun 😃
.NET Core can also run on devices such as Raspberry Pi with NET Core 3.0 SDK - Linux ARM32 version. Any IoT scenario can be done with .NET Core 3.0 such as getting some sensor data, setting a device and monitor an environment....etc.

So, I run application this application on Raspberry Pi 3 with OLED display. So even if new earthquake data is fetched it is displayed on OLED at device. The display stuff is not related with .NET Core 3.0, it is just one of my previous python code with HTTP server feature to get input. Don't forget to check...

Raspberry Pi Device

Azure IoT Central

I just wantted to dig a little bit to learn more about Azure IoT Central so this application also registered as a IoT device and send data to Azure IoT Central endpoints to monitor device. Also review this code in IoT aspect...

Simplify IoT development—from setup to production Build production-grade IoT applications in hours, without managing infrastructure or relying on advanced IoT development skills. Reduce the complexity of customizing, deploying, and scaling an IoT solution—and bring your connected solutions to market faster.

Briefly, Azure IoT Central is a very good platform to manage IoT devices; device management, device provisioning can be done easily.

To dig more about to topics in this repository, please check followings;

.NET Core 3.0 BackgroundService, IHostedService
Azure IoT Central
systemd services
Unit files
Happy coding ❤️
