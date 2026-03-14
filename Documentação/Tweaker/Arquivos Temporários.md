

```
        private void button1_Click(object sender, EventArgs e)
        {
            try
            {
                string psCommand = @"
                Remove-Item 'C:\Windows\Temp\*' -Recurse -Force -ErrorAction SilentlyContinue
                Remove-Item '$env:TEMP\*' -Recurse -Force -ErrorAction SilentlyContinue
                Remove-Item 'C:\Windows\Prefetch\*' -Recurse -Force -ErrorAction SilentlyContinue
                ";

                ProcessStartInfo psi = new ProcessStartInfo();
                psi.FileName = "powershell.exe";
                psi.Arguments = $"-NoLogo -NoProfile -Command \"{psCommand}\"";
                psi.Verb = "runas";
                psi.UseShellExecute = true;

                Process.Start(psi);

                MessageBox.Show("Arquivos temporários apagados\n", "Sucesso", MessageBoxButtons.OK, MessageBoxIcon.Information);

            }
            catch(Exception ex)
            {
                MessageBox.Show("Erro ao apagar arquivos temporários\n" + ex.Message, "Falha", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }
```