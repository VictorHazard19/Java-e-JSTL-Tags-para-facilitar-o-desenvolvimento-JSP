# JSTL 

## 1.1 - Conceito

- JSPs cuidam da parte visual de um projeto Java Web

- Nos arquivos JSP, usamos Java para dar alguma inteligência e dinamismo ao HTML de nosso projeto.

- O grande problema de misturar Java e HTML no JSPs é a manutenção do código após a implementação do projeto.

- A JSTL tem o intuito de auxiliar no desenvolvimento de páginas dinamicas, usando menos código Java e adequar o código às Tags do HTML

- A JSTL é definido com a marcação de tags, semelhante ao HTML - mantendo a familiaridade do código.

- Para a JSP identificar que as tags da JSTL são código Java e para que ela entenda que aquelas tags são diferenciadas, é necessário realizar os devidos imports no topo da JSP:

 <%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
 
 * Não é um endereço, apena o nome da biblioteca **core** da JSTL
 * Toda vez que usamos o Core da JSTL, o prefixo usado é o **c**
 
 - A JSTL trabalha em conjunto com a El - Expression Language
 
 
 Para realizar um looping:
 
 <c:forEach var="nomeVariavel" items=${nomeAtributoRequest}>
   <tr>Lista de Itens>
	<td>${nomeVariavel.id}</td>  //Não é necessário chamar o método get do objeto, a EL já se encarrega de chamar o método get do atributo apenas com a indicação de qual será usado.
 <c:/forEach> 
 
 
 Definindo um contador no forEach:
 
 <c:forEach var="p" items="${produtoList}" varStatus="st">
  ${st.count}
 </c:forEach>
 
 Para realizar a validação de uma condição usando if:
 
 
	<c:if test="${p.usado}"><td>Sim</td></c:if>
	<c:if test="${not p.usado}"><td>Não</td></c:if>
	
	* Não existe um **c:else**. Para validar a condição contrária, é preciso utilizar um outro c:if e na EL utilizar o operado **not** para negar a condição

 Para tentar contornar o problema da falta de else na estrutura c:if, é possível utilizar uma estrutura semelhante ao switch do java:
 
 <c:choose>
	<c:when test="${p.usado}">
		<td>Sim</td>
	</c:when>
	<c:otherwise>
		<td>Não</td>
	</c:otherwise>				
</c:choose>

	O c:choose valida a condição dada em c:when. Caso nenhuma das condição seja encontrada nos N c:when da validação, o valor definido em c:otherwise é o padrão.
	
 - Para URLs, usa-se o c:url, ela auxilia na criação de URLs, descobrindo e repassando ao JSP o context root da aplicação 
 (O context root pode mudar de um ambiente para o outro 
	
