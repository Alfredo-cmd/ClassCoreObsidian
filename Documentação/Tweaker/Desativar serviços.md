
```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Text;
using System.Windows.Forms;

namespace SystemTweaker
{
    public partial class Perfomance : Form
    {
        Form1 fp1;
        public Perfomance(Form1 fp)
        {
            InitializeComponent();
            fp1 = fp;
        }
        private void DisableServices()
        {
            string psCommand = @"
                    $services = @(
                        'TapiSrv',
                        'SysMain',
                        'WSearch',
                        'WpcMonSvc',
                        'WbioSrvc',
                        'RemoteRegistry',
                        'DiagTrack'
                    )

                    foreach ($serv in $services) {
                        if (Get-Service -Name $serv -ErrorAction SilentlyContinue) {
                            Stop-Service -Name $serv -Force -ErrorAction SilentlyContinue
                            Set-Service -Name $serv -StartupType Disabled
                        }
                    }
                    ";
            try
            {
                fp1.ProcessStart(psCommand);
            }
            catch (Exception ex)
            {
                MessageBox.Show("Erro ao desativar serviços\n" + ex.Message, "Falha", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }

        private void btn_DisableServices_Click(object sender, EventArgs e)
        {
            DisableServices();
        }
    }
}

```