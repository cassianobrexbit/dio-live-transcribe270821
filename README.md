# dio-live-transcribe270821
Repositório de código para a live sobre o Amazon Transcribe da Digital Innovation One

### Recursos AWS utilizados na atividade:

- IAM Role para trigger do Amazon Transcribe.
- Função Lambda.
- Buckets de entrada e saída de arquivos no S3.

### Desenvolvimento

#### Criar Role no IAM com permissões para o Transcribe

- IAM Dashboard -> Create Role -> AWS [Service Lambda] -> Policies [CloudWatchLogsFullAccess e AmazonTranscribeFullAccess] -> Inserir um nome -> Create new Role.

#### Criar função lambda

- Lambda dashboard -> Runtime Python3.x -> Permission [Using an existing role - nome da função lambda criada no passo anterior]
- Copiar o código ```src/transcribeFunction.js```
A função Lambda executa os seguintes passos quando disparada:
- Será disparada por um evento do S3.
- A função irá criar um nome de job requerido pela API do Transcribe, para ser único um UUID será concatenado ao nome.
- A função irá chamar o método ```start_transcription_job``` com os parâmetros necessários. As variáveis ``` <seu_bucket_de _entrada> ``` e ```<seu_bucket_de_saida>``` devem ser editados com um nome único global.

#### Criando buckets de entrada e saída e o evento para disparar o gatilho da função lambda

 - S3 Dashboard -> Create bucket -> Selecionar região -> Manter o restante as configurações padrão -> Create bucket.
 Repetir a operação para o bucket de saída.
 
#### Atribuindo permissões de escrita para o bucket S3 de saída

- IAM Dashboard -> Policies -> Create Policy -> Select a service -> Choose a service [S3] -> Actions [List -> ListBucket] - [Write -> PutObject] -> Resources -> Specific
  - bucket: ```arn:aws:s3:::<seu_bucket_de_saida>``` .
  - object: ```arn:aws:s3:::<seu_bucket_de_saida/*>```. 
- Next -> Next -> Name -> Create Policy.

Adicionar a política criada à função Lambda

- IAM Dashboard -> Roles -> Selecionar a role criada -> Attach Policies -> Buscar a Policy criada anteriormente -> Attach policy

#### Criando o evento no S3 para disparar a função ambda

 - S3 Dashboard -> Selecionar o bucket de entrada dos arquivos -> Event notifications -> Create event notification
   - Inserir um nome
   - All object create events
   - Destination -> Lambda function -> selecionar a função lambda criada
   - Save changes

#### Executando a aplicação

- Acessar o site https://freetts.com/Home/PortugueseTTS e gerar um arquivo de áudio por meio de palavras digitadas. É o oposto do Transcribe nesse caso.
- Fazer o download do arquivo.
- Acessar o S3 e fazer o upload do arquivo.
- Acessar o console do Transcribe para acompanhar o status da execução do job de transcrição.
- Acessar o bucket de saída e realizar o download do arquivo JSON com a transcrição.
