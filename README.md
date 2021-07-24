# tarefas1
tarefas em background-jobs-class
### configurações:
 exportar  {  default  as  RegistrationMail  }  de  './RegistrationMail' ;

####  user controler
importar  passwordGenerator  de  'password-generator' ;

import  Queue  from  '../lib/Queue' ;

export  default  {
   armazenamento assíncrono ( req ,  res )  {
    const  { nome , email }  =  req . corpo ;

    const  user  =  {
      nome ,
      email ,
      senha : passwordGenerator ( 15 ,  falso ) ,
    } ;

    aguarde a  fila . adicionar ( 'RegistrationMail' ,  { usuário } ) ;

    retornar  res . json ( usuário ) ;
  }
} ;

####  registration mail
importar  correio  de  '../lib/Mail' ;

export  default  {
  chave : 'RegistrationMail' ,
  opções : {
    atraso : 5000 ,
    prioridade : 3 ,
    repetir : {
      a cada : 1 ,
      limite : 100
    } ,
    lifo : verdadeiro
  } ,
   identificador assíncrono ( { data } )  {
    const  { usuário }  =  dados ;

    aguarde o  correio . sendMail ( {
      de : 'DIO <contato@dio.com.br>' ,
      para : ` $ { usuário . nome } < $ { usuário . email } > ` ,
      assunto : 'Cadastro de usuário' ,
      html : `Olá, $ { usuário . nome } , bem-vindo a DIO.
    } ) ;
  } ,
} ;

####  index.js
 exportar  {  default  as  RegistrationMail  }  de  './RegistrationMail' 
 

####  Mail.js
importar  nodemailer  de  'nodemailer' ;
importar  mailConfig  de  '../../config/mail' ;

exportar  nodemailer padrão  . createTransport ( mailConfig ) ;

####  Queue.js
import  'dotenv / config' ;

import  Queue  from  './app/lib/Queue' ;

Queue . processo ( ) ; 
#### Queue.js;
  
importar  fila  de  'touro' ;
importar  redisConfig  de  '../../config/redis' ;

import  *  as  jobs  de  '../jobs' ;

const  queues  =  Object . valores ( empregos ) . mapa ( trabalho  =>  ( {
  bull : nova  fila ( job . key ,  redisConfig ) ,
  nome : trabalho . chave ,
  identificador : trabalho . alça ,
  opções : trabalho . opções ,
} ) )

export  default  {
  filas ,
  adicionar ( nome ,  dados )  {
     fila  const =  isso . filas . encontrar ( fila  =>  fila . nome  ===  nome ) ;
    
     fila de retorno . touro . adicionar ( dados ,  fila . opções ) ;
  } ,
  processo ( )  {
    devolva  isso . filas . forEach ( fila  =>  {
      fila . touro . processo ( fila . identificador ) ;

      fila . touro . on ( 'falhou' ,  ( trabalho ,  err )  =>  {
        console . log ( 'Trabalho falhou' ,  fila . chave ,  trabalho . dados ) ;
        console . log ( errar ) ;
      } ) ;
    } )
  }
} ;

####  server
import  'dotenv / config' ;
importar  expresso  de  'expresso' ;
importar  BullBoard  de  'bull-board' ;

importar  UserController  de  './app/controllers/UserController' ;
import  Queue  from  './app/lib/Queue' ;

const  app  =  express ( ) ;
BullBoard . setQueues ( Queue . queues . map ( queue  =>  queue . bull ) ) ;

app . usar ( express . json ( ) ) ;
app . post ( '/ users' ,  UserController . store ) ;

app . use ( '/ admin / queues' ,  BullBoard . UI ) ;

app . ouvir ( 8080 ,  ( )  =>  {
  console . log ( 'Servidor rodando na porta 8080' ) ;
} ) ;

####  configurações
####  mail
export  default  {
  host : processo . env . MAIL_HOST ,
  porta : processo . env . MAIL_PORT ,
  auth : {
    usuário : processo . env . MAIL_USER ,
    passar : processo . env . MAIL_PASS
  } ,
} ;
####  redis.js
export  default  {
  host : processo . env . REDIS_HOST ,
  porta : processo . env . REDIS_PORT ,
} ;

<git.push origin red. desafio>
