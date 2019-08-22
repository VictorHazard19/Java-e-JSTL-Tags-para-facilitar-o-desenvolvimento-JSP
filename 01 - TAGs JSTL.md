# JSTL 

  [Documentação JSTL](https://docs.oracle.com/javaee/5/jstl/1.1/docs/tlddocs/)

## 1.1 - Conceito

- JSPs cuidam da parte visual de um projeto Java Web

- Nos arquivos JSP, usamos Java para dar alguma inteligência e dinamismo ao HTML de nosso projeto.

- O grande problema de misturar Java e HTML no JSPs é a manutenção do código após a implementação do projeto.

- A JSTL tem o intuito de auxiliar no desenvolvimento de páginas dinamicas, usando menos código Java e adequar o código às Tags do HTML

- A JSTL é definido com a marcação de tags, semelhante ao HTML - mantendo a familiaridade do código.

- Para a JSP identificar que as tags da JSTL são código Java e para que ela entenda que aquelas tags são diferenciadas, é necessário realizar os devidos imports no topo da JSP:
```java
 <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
```
 
 - Não se trata de um endereço, apena o nome da biblioteca **core** da JSTL
 
 - Toda vez que usamos o Core da JSTL, o prefixo usado é o **c**
 
 - Para formatação, usa-se o pacote fmt com o prefixo de mesmo nome:
 
 ```java
  <%@ taglib uri="http://java.sun.com/jsp/jstl/fmt" prefix="fmt"%>
```

 - O pacote FMT é conjunto de tags que auxiliam na formatação do código.

 - Com o pacote fmt é possível formatar a data, valores e etc. 
 
 - A JSTL trabalha em conjunto com a **El** - Expression Language
 
 ## 1.2 - Principais TAGs
 
 ### Para realizar um looping:
 
 ```java
 <c:forEach var="nomeVariavel" items=${nomeAtributoRequest}>
   <tr>Lista de Itens>
	<td>${nomeVariavel.id}</td>  //Não é necessário chamar o método get do objeto, a EL já se encarrega de chamar o método get do atributo apenas com a indicação de qual será usado.
 <c:/forEach> 
 ```
 
 ### Definindo um contador no forEach:
 
 ```java
 <c:forEach var="p" items="${produtoList}" varStatus="st">
  ${st.count}
 </c:forEach>
 ```
 
### Para realizar a validação de uma condição usando if:
 
```java 
 <c:if test="${p.usado}"><td>Sim</td></c:if>
 <c:if test="${not p.usado}"><td>Não</td></c:if>
 ```
	
- Não existe um **c:else**. Para validar a condição contrária, é preciso utilizar um outro **c:if** e na *EL* utilizar o operado **not** para negar a condição

 Para tentar contornar o problema da falta de else na estrutura c:if, é possível utilizar uma estrutura semelhante ao switch do java:
 
 ### Switch
 
 ```java 
 <c:choose>
	<c:when test="${p.usado}">
		<td>Sim</td>
	</c:when>
	<c:otherwise>
		<td>Não</td>
	</c:otherwise>				
</c:choose>
```
 - O **c:choose** valida a condição dada em **c:when**. Caso nenhuma das condição seja encontrada nos *N c:when* da validação, o valor definido em **c:otherwise** é o padrão.

### Criando URLs

  - Para URLs, usa-se o **c:url**, ela auxilia na criação de URLs, descobrindo e repassando ao JSP o context root da aplicação 
 (O context root pode mudar de um ambiente para o outro)
 
 - Contexto é o "começo" da URL. Aquele que diz qual projeto deve ser executado - **www.meusite.com/produtos**
 
 - A JSTL sempre sabe como gerar a URL
 
 - A **c:url** recebe um link ( */novaURL* - o context root fica implicito nessa declaração) e uma variável que receberá este link
 
 ```java 
 <c:url value="/produto/formulario" var="novaURL"/>
	<a href="${novaURL}">Adicionar um produto</a>
```
	
 - É possível escreve a **c:url** *inline* - diretamente no *href*. Basta não criar nenhuma variável na declaração da TAG.

```java 
    <a href="<c:url value='/produto/formulario'></c:url>">Adicionar um produto</a>
   ```

### Definindo Variáveis

- A tag "set" serve para definir uma variável. Ela é útil quando temos um valor que será repetido por todo o programa, e guardar numa variável pode ser importante.

- A tag "out" imprime o conteúdo da variável. Seu comportamento é idêntico ao da EL: ${nome}.

```java 
    <c:set var="nome" value="João da Silva" />
    <c:out value="${nome}" />
   ```

### Formatando Datas

- É possível formar a data utilizando-se de diverso patterns:

```java 
    <td><fmt:formatDate value="${p.dataInicioVenda.time}" pattern="dd/MM/yyyy"/></td>
    // 22/08/2019
   ```
   
 ```java 
    <td><fmt:formatDate value="${p.dataInicioVenda.time}" pattern="EEEE, dd 'de' MMMM 'de' yyyy"/></td>
    // Quinta-feira, 22 de Agosto de 2019
   ```

 ### Formatando Números
 
 - Da mesma maneira, é possível formatar números utilizando a JSTL, EL e patterns
 
  ```java 
    <td><fmt:formatNumber value="${p.preco}" type="currency"/></td>
    // Quinta-feira, 22 de Agosto de 2019
   ```
 - Ela é realmente bem útil e prática. Se fôssemos fazer isso na mão com Java, gastaríamos algumas linhas para tal; com a JSTL isso fica escondido, e nossa JSP precisa agora só de 1 linha para formatar o número.
 
 ### Internacionalização

- Outro grande recurso da JSTL é a internacionalização e a centralização das mensagens da aplicação

- Primeiro é necessário 'avisar' a JSTL que teremos um arquivo de configuração para a internacionalização - Adicionar ao **web.xml** o seguinte contexto:

 ```java
  <context-param>
    <param-name>
      javax.servlet.jsp.jstl.fmt.localizationContext
    </param-name>
    <param-value>messages</param-value>
  </context-param>
  ```
  
- O arquivo **messages** (pode ser qualquer nome) deverá ter a extensão **.properties** e conterá todas as palavras que serão exibidas pela aplicação.

- Ele deve ficar na pasta **src** do projeto.

- O arquivo será um conjunto de chaves e valores (Chaves são as referências que serão pesquisadas pela JSTL e os Valores são os textos que serão exibidos) *Nome de chaves não podem ter espaço.*

- Para incluir um texto na aplicação, utilizamos a tag **message** da JSTL:

 ```java
   <fmt:message key="mensagem.bemvindo" />
 ```
- Caso a JSTL não encontre o arquivo de uma liguagem especifica, ele utiliza o arquivo padrão (nomeArquivo.properties - No caso, messages.properties)

- Para internacionalizar a aplicação, basta criar uma cópia do arquivo e renomea-la com o mesmo prefixo (no caso **messages**) e adiciona **_lingua** (_EN, _UK, _DE, _IT ....)

- Neste novo arquivo, deve-se manter as mesmas chaves e alterar os valores para a língua em questão.

- A JSTL vai validar qual a linguagem do browser onde a aplicação está rodando e vai tentar procurar o arquivo equivalente.

 (Se o browser está em inglês, ele vai buscar o arquivo messages_en.properties
  Se estiver em alemão, tenta buscar o arquivo messages_de.properties)
  
- Caso ele não encontre o arquivo equivalente, ele utiliza o arquivo padrão **messages.properties**

### Reutiização de Páginas

- Para reutilizar uma página (um rodapé por exemplo) e não ter que colar o trecho dessa reutilização em todas as paginas, basta utilizar a tag **c:import**

 ```java
 <c:url value"/caminho/pagina.jsp"/>
 ```
 
- tag muito útil para a criação de menus, rodapés e cabeçalhos.
