
É sempre importante dar pelo menos a opção para o usuário de criar esse ponto
dessa forma é possivel reverter qualquer coisa prejudicial feita pelas outras funções do programa.


## Como implementar?

O Csharp sozinho não consegue criar um ponto de restauração, porém através do uso do powershell, é possivel.


## Primeiro passo: 

-Entender os requisitos para a criação: 
- Permissão de administrador.
- Proteção do sistema ativada no disco
- W10 / W11

Comando no powershell:

`Checkpoint-Computer -Description "TestePontoRestauracao" -RestorePointType "MODIFY_SETTINGS"`


## Segundo passo: Implementar no código


Primeiro, tenho que escrever 
```
using System.Diagnostics
```
Esse namespace vai possibilitar eu usar classes como Process para executar coisas fora do meu programa.


Código + Explicações:
```
private void btn_restorePoint_Click(object sender, EventArgs e)
{
    try
    {
        //Classe de config, onde armazena as permissoes, parametros etc, tipo um formulario
         ProcessStartInfo psi = new ProcessStartInfo();

        //executar o powershell, como é um programa já do windows não preciso especificar caminho
        psi.FileName = "powershell.exe";

       DialogResult result = MessageBox.Show(
            "Deseja escolher um nome?\nCaso não, um nome padrão será usado.",
            "Nome",
            MessageBoxButtons.YesNo,
            MessageBoxIcon.Question
            );
            
        //Abre outro form para o usuario escrever o nome desejado
        if (result == DialogResult.Yes)
        {
            PointName pointName = new PointName(this);
            pointName.ShowDialog();

        }
        
        //É oque eu digitaria manualmente no powershell
        psi.Arguments = $"Checkpoint-Computer -Description \"{namePoint}\" -RestorePointType \"MODIFY_SETTINGS\"";
        //Define que o windows shell vai ser usado, sem ele o comando psi.Verb=runas nao funcionaria.
        psi.UseShellExecute = true;
        //Força a usar o administrador
        psi.Verb = "runas";
        psi.CreateNoWindow = true;

        Process.Start(psi);

        MessageBox.Show("Ponto de restauração solicitado com sucesso", "Sucesso", 
            MessageBoxButtons.OK, MessageBoxIcon.Information);



    }
    catch(Exception ex)
    {
        MessageBox.Show("Erro ao criar o ponto de restauração\n" + ex.Message,
            "Erro", MessageBoxButtons.OK, MessageBoxIcon.Error);
    }

}
```