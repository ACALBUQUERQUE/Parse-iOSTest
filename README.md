Teste iOS (Parse + Google Places API)
===================

Este teste foi desenvolvido para testar as habilidades de código utilizando dados hospedados no Parse(parse.com) e Google Places API.

Trata-se basicamente de um app que lista algumas cidades através do Parse, busca de restaurantes dessa cidade através do Google Places API exibindo-os em um Mapa e uma tela de detalhes do local selecionado com os comentários sobre o restaurante selecionado.

É um teste para desenvolvedores de **Nível Pleno** e o tempo máximo para execução do teste é de **8 horas**. 

Mesmo que você não tenha terminado, não deixe de envia-lo até este limite.

----------


Objetivos
-------------
O objetivo deste teste é verificar os conhecimentos dos candidatos consumindo dados hospedados no Parse, na Google Places API e a estruturação desses dados.

> **Note:**

> - Não será avaliada a sua capacidade de design, mas sim a de código. Você não precisa se preocupar com o visual, mas com a construção, performance e adaptabilidade do layout;
> - Você poderá consumir os dados do Parse via SDK ou via REST. É desejável o uso do SDK para o consumo destes dados. Entretanto caso você não conheça o iOS SDK do Parse, você poderá utilizar a REST API. Mais Detalhes abaixo.
> - Todas as etapas deste projeto podem ser feitas utilizando as ferramentas e recursos nativos do iOS. Entretanto, caso você conheça e opte usar a  **AFNetworking** para consumir os dados via REST, isto não contará negativamente no seu teste. **Esta é a única lib de terceiros permitida!**

Requisitos
-------------------
Os requisitos obrigatórios para este teste são:

- Tela com a lista de todas as cidades disponibilizadas pela classe no Parse.
- Na seleção de uma das cidades, consultar a Google Places API e mostrar os resultados em um mapa.
- Ao clicar num desses resultados, mostrar uma tela com o nome do local, endereço, rating e foto do local (todos fornecidos pela Google Places).
- Na mesma tela de detalhes do local, ter um botão para exibir uma tela com os comentários existentes no Parse deste local usando como parâmetro de busca o **place_id**. Esta tela com os comentários deverá exibir tanto os comentários em si quanto a foto(se existir) deste comentário. Caso não existam comentários sobre o local, exibir um alerta para o usuário que não existem comentários.

**Sugestão:** Na tela com comentários do local, usar uma tableview com uma célula customizada(foto+comentário ou somente comentário). Tela semelhante ao instagram.

**- Atividades bônus (opcionais):**
Criar um botão nos detalhes do local para exibir uma view que possibilite escrever um comentário, tirar uma foto e envia-los para o parse.

*PS: Esta tarefa só poderá ser feita se o SDK iOS do Parse for utilizado!

Classes Parse
-------------------
Este teste possui 2 classes (ou tabelas) hospedadas no [Parse][1].

#### <i class="icon-file"></i> Cidades
Nesta classe a única variável de interesse é a **nome** pois será ela que preencherá a célula da tableview na listagem de cidades.

**iOS SDK**

Para acessar estar classe **via SDK**, será necessário se obter o **AppID** e o **clientKey** que serão enviados por email. 
Após isso, basta configurar o parse no appDelegate e fazer uma consulta na ViewController usando Cidades como classe.

**-Exemplo:** 

    // AppDelegate.m
    // Setting Parse Configs
    [Parse setApplicationId:@"appID" clientKey:@"clientKey"];

    // viewcontroller.m 
    PFQuery *query = [PFQuery queryWithClassName:@"Cidades"];

Mais Detalhes sobre a instalação e uso do Parse iOS SDK [Aqui][2] e [Aqui][3]

**REST API**

Para acessar esta classe **via REST**, a url de consulta terá este formato as chaves disponibilizadas deverão substituir o **ParseAppID** e **ParseJavaScriptKey**:

<https://ParseAppID:javascript-key=ParseJavaScriptKey@api.parse.com/1/classes/Cidades>

**-Exemplo Retorno JSON :**

    {
	    results: [
		    {
			    createdAt: "2015-05-11T19:27:37.500Z",
			    nome: "Recife",
			    objectId: "6eqxEej8CG",
			    updatedAt: "2015-05-11T19:27:37.500Z"
		    },
		    {
			    createdAt: "2015-05-11T19:27:57.721Z",
			    nome: "Campinas",
			    objectId: "LRiJn7xqrw",
			    updatedAt: "2015-05-11T19:38:08.160Z"
		    },
		    ...
	    ]
    }

#### <i class="icon-file"></i> Comentários
Esta classe possui 3 variáveis importantes para o projeto:

- **place_id** - Id do local no Google Places API (string)
- **comentario** - Texto do comentário (string)
- **foto** - Arquivo da imagem (PFFile)

Para acessar os dados via SDK ou REST, basta seguir os exemplos citados na classe de **Cidades** modificando apenas o nome da classe para **Comentarios**.

**-Retorno JSON da classe de Comentários:**

    {
	    results: [
		    {
			    createdAt: "2015-05-11T19:27:37.500Z",
			    comentario: "Restaurante bom e barato.",
			    objectId: "6eqxEej8CG",
			    updatedAt: "2015-05-11T19:27:37.500Z",
			    place_id: "ChIJ1YD6q7ofqwcReGnCNJq0qzM",
			    foto: {
				    __type: "File",
				    name: "tfss-2e0c0cfe-65c9-4098-981e-d4f747035a4e-rest1.jpg",
				    url: "http://files.parsetfss.com/42631dd9-4954-4168-afb9-d748442ce6c5/tfss-2e0c0cfe-65c9-4098-981e-d4f747035a4e-rest1.jpg"
				    }
		    },
		    {
			    createdAt: "2015-05-11T19:27:37.500Z",
			    comentario: "Restaurante fino e caro.",
			    objectId: "CzxEGBPrlX",
			    updatedAt: "2015-05-11T19:27:37.500Z",
			    place_id: "ChIJz6Gqq-cYqwcRnuPcfC098HQ",
			    foto: {
				    __type: "File",
				    name: "tfss-51b3f5c7-b2bd-4cd5-8286-cb3e46111936-rest2.jpg",
				    url: "http://files.parsetfss.com/42631dd9-4954-4168-afb9-d748442ce6c5/tfss-51b3f5c7-b2bd-4cd5-8286-cb3e46111936-rest2.jpg"
				    }
		    },
		    ...
	    ]
    }

Google Places API
-------------------
Para consultar o JSON que lista restaurantes de uma cidade, basta usar a seguinte URL de exemplo e substituir **GPlacesKey** pela chave disponibilizada e **Cidade** pelo nome da cidade escolhida: 

https://maps.googleapis.com/maps/api/place/textsearch/json?query=restaurante+Cidade&sensor=false&key=GPlacesKey
 
Mais detalhes deste serviço pode ser encontrado [aqui][4].

Para capturar a foto do local, será necessário o uso de um segundo WebService da Google usando a chave **photo_reference** disponibilizada no JSON anterior de places. 
Caso não exista a variável **photos** que é um dicionário que contém a variável **photo_reference**, significa que o google não possui foto deste local e não dá para consultar a foto.

Exemplo:

    ...
    photos:[{
	    html_attributions: ["From a Google User"],
	    height: 240,
	    width: 320,
	    photo_reference: "CoQBdQAAAF3BAS4RzMM0Wkm4gO-Ep1SiYLjExdzdXBBEPJRnc-K47CgWNEFIpG1a8sWbaggnuEYlfddQJXjulYGgtsLUvIb5pNHo_qPgEm0Nz0ro_qUkrN5P2ezjhYZDEDWsn15l4RSzjA2izKcBlT8gz7n13H3JRHi9M35oPnqiLs7IODOAEhDVhF7WNUihYgCKIwVvv0i3GhSE_ePxLRcC8gt_8g7T3QknbSQfWg"
	    }],
	...
	    


A URL base do segundo webservice é:

https://maps.googleapis.com/maps/api/place/photo?parameters

**Parâmetros:**

- **maxwidth** : Recebe valores inteiros entre 1 e 1600 e especifica o tamanho máximo da foto.
- **photoreference** : Hash String do local fornecida pelo JSON de places
- **key** : Chave da API que será disponibilizada.

Exemplo da URL:  
https://maps.googleapis.com/maps/api/place/photo?maxwidth=400&photoreference=photo_reference&key=GPlacesKey

Mais detalhes desse WS pode ser visto [aqui][5].

Procedimentos
-------------------
 1. Clonar este projeto p/ sua conta GitHub.
 2. Implementar a interface conforme exemplo do diretório Design. O design deve ser adaptado para ser executado no iPhone.
 3. Para consultar os dados, tanto do Parse e Google Places, será necessário ter as chaves de acesso de cada API. Estas serão enviadas no email de resposta após o envio do email para <toni@madeinweb.com.br> confirmando o interesse no processo de seleção.

PS: O tempo começará a ser contabilizado no momento em que você receber este email com as chaves de acesso.

  [1]: http://parse.com/
  [2]: https://www.parse.com/apps/quickstart#parse_data/mobile/ios/native/existing
  [3]: https://www.parse.com/docs/ios/guide
  [4]: https://developers.google.com/places/webservice/details
  [5]: https://developers.google.com/places/webservice/photos
