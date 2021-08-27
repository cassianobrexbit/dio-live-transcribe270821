# dio-live-transcribe270821
Repositório de código para a live sobre o Amazon Transcribe da Digital Innovation One

### Recursos AWS utilizados na atividade:

- IAM Role para trigger do Amazon Transcribe
- Função Lambda
- Buckets de entrada e saída de arquivos no S3

### Desenvolvimento

#### Criar Role no IAM com permissões para o Transcribe

- IAM Dashboard -> Create Role -> AWS [Service Lambda] -> Policies [CloudWatchLogsFullAccess e AmazonTranscribeFullAccess] -> Inserir um nome -> Create new Role

#### Criar função lambda

- Lambda dashboard -> Runtime Python3.x -> Permission [Using an existing role - nome da função lambda criada no passo anterior]
- Copiar o código ```src/transcribeFunction.js```
A função Lambda executa os seguintes passos quando disparada:
- Será disparada por um evento do S3.
- A função irá criar um nome de job requerido pela API do Transcribe, para ser único um UUID será concatenado ao nome.
- A função irá chamar o método ```start_transcription_job``` com os parâmetros necessários. As variáveis ``` <seu_bucket_de _entrada> ``` e ```<seu_bucket_de_saida>``` devem ser editados com um nome único global.

#### Criando buckets de entrada e saída e o evento para disparar o gatilho da função lambda

 - S3 Dashboard -> Create bucket -> Selecionar região -> Manter o restante as configurações padrão -> Create bucket
 Repetir a operação para o bucket de saída
 
#### Atribuindo permissões de escrita para o bucket S3 de saída

- IAM Dashboard -> Policies -> Create Policy -> Select a service -> Choose a service [S3] -> Actions [List -> ListBucket] - [Write -> PutObject] -> Resources -> Specific
  - bucket: ```arn:aws:s3:::<seu_bucket_de_saida>``` 
  - object: ```arn:aws:s3:::<seu_bucket_de_saida/*>``` 
- Next -> Next -> Name -> Create Policy
