# AtWS - Autoridade Tributária WebService


### Sobre Este Ficheiro ###
DLL com as funções de encriptação e envio por webservices de documentos 
de transporte e documentos de faturação para o servidor da AT
Programa demonstração de como usar a DLL

#### Testado com o serviço de envio de documentos de transporte e com o 
serviço de envio de faturas, mas é possível que funcione também com outros 
webservices da Autoridade Tributária


### Change Log: ###

**v2017.11.10**: Optimizações

**v2017.10.27**: Actualização da Chave Pública da AT 
(ChavePublicaAT.pem --> ChaveCifraPublicaAT2020.pem)

**v2017.8.23**: Actualização de alguns **Reusable Objects**, efectivamente 
removemendo dependências nos seguintes módulos: **DelphiOnRails**, 
**SuperObject** e **UIB**

**v2017.08.22**: Remoção da dependência da **CapiCOM**, remoção da 
dependência da chave pública **ChavePublicaAT.pem**

**v2013.12.11**: Remoção da necessidade de ter o certificado instalado no 
Windows


### NOTAS: ###

* A CapiCOM deixou de ser necessária a partir da versão v2017.08.22

* Não é necessário instalar o certificado no Windows a partir da versão 
v2013.12.11


### QUESTÕES CONHECIDAS: ###

Embora este projecto dispense que o certificado esteja instalado na máquina 
cliente, o certificado de raíz DGITA Issuing CA (DGITA Root CA) precisa estar 
instalado (ver Issue #2).


### ATENÇÃO: ###

Esta é uma DLL de testes para fins meramente didáticos. Não foi alvo de testes 
intensivos que seriam necessários para ambiente de produção. Usem por vosso risco.


### Funções: ###

#### function ATWebService(const SoapURL, SoapAction, PubKeyFile, PFXFile, 
####  PFXPass: WideString): IAtWSvc; 
* Cria um objecto de envio de documentos para a AT por webservice
* Aceita por parâmetro 5 strings, com encoding UniCode
* **SoapURL** é o *URL* do webservice
* **SoapAction** é a *Action* do webservice
* **PubKeyFile** é o caminho do ficheiro **ChavePublicaAT.pem**. Se o caminho 
for uma string vazia, ou o ficheiro indicado não existir, a dll tentará usar 
a chave pública embutida
* **PFXFile** é o caminho do ficheiro do certificado PFX a usar
* **PFXPass** é a password do certificado
* Retorna um *interface* **IAtWSvc**, cuja declaração pode ser encontrada no 
ficheiro **AtWSvcIntf.pas**
* A *Naming Convention* é ***StdCall***, standard do Windows.


### IAtWSvc: ###

O *interface* IAtWSvc possui apenas um método declarado:
```delphi
  IAtWSvc = interface
  ['{5119FAB4-F5BB-458E-99F0-96CF75F58981}']
    function Send(const XMLData: WideString): WideString;
  end;
```

* **Send** aceita por parâmetro uma string com o pedido ao webservice em formato 
**XML**, e retorna a resposta do webservice, também em formato **XML**.


### Pedido XML: ###

O **XML** de pedido ao webservice deve estar no formato indicado pelo manual de 
comunicação de documentos da Autoridade Tributária, com as seguintes excepções:
* **Nonce** O campo **Nonce** deverá existir, mas sem conteúdo: 
`<wss:Nonce></wss:Nonce>`
* **Password** O campo **Password** deverá ir preenchido com a password sem 
qualquer encriptação. Será o objecto a encriptar essa password com o formato 
exigido pelo manual de comunicação de documentos da Autoridade Tributária: 
`<wss:Password>123456789</wss:Password>`
* **Created** O campo **Created** deverá existir, mas sem conteúdo: 
`<wss:Created></wss:Created>`


### Requisitos: ###

* É preciso o certificado, de produção ou de testes, atribuído pela AT.

* O ficheiro **ChavePublicaAT.cer**/**ChaveCifraPublicaAT2020.cer** não é necessário, 
pois a chave pública está *hardcoded* na dll. De qualquer forma, é possível passar-lhe 
a chave pública, caso a versão *hardcoded* expire e não se deseje recompilar a dll. 
Para isto, deve ser passado o ficheiro convertido no formato **PEM** 
(**ChavePublicaAT.pem**/**ChaveCifraPublicaAT2020.pem**).

* Este projecto recorre a algumas dependências para executar os seus objectivos. São 
dependêncais de compilação, pelo que não são necessárias para usar directamente os 
ficheiros binários presentes no projecto (lib), mas são necessárias para a sua 
recompilação.
  - Reusable Objects (https://github.com/nunopicado/Reusable-Objects)


### AVISO: ###

De notar que esta DLL não foi alvo de testes extensivos, pelo que o seu uso é por 
conta e risco do utilizador.
Eu uso-a, com algumas alterações para fazer face às minhas necessidades, e nunca 
me deu problema, mas ainda assim, não posso garantir nada.
