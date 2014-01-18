---
layout: post
title: HTMLUnit et JSFUnit : tester la couche de présentation
description: ""
category: articles
tags: [java,tests,development]
---

[HTMLUnit](http://htmlunit.sourceforge.net/) est une extension de [JUnit](http://www.junit.org/) qui propose de tester une application *live*, c'est à dire en permettant au développeur d'exécuter des tests d'application de bout en bout. Ce *framework* se décrit comme un navigateur web sans interface puisqu'il offre la possibilité d'appeler les éléments HTML de pages et ainsi de simuler une navigation. Le développeur peut donc remplir des formulaires, cocher des *checkbox*, renseigner des champs et ensuite simuler un clic sur un bouton, un lien ou valider un formulaire.

HTMLUnit peut être directement utilisé comme une extension de JUnit. Pour ce faire, il suffit d'inclure les JARs nécessaires dans le CLASSPATH pour avoir accès aux fonctionnalités de test web. Ci-après un exemple dans lequel on retrouve l'annotation `@Test` qui témoigne de l'utilisation de JUnit. Celui-ci invoque une page via la classe WebClient et en vérifie le nom ainsi que certains éléments constitutifs.

`@Test public void homePage() throws Exception {   final WebClient webClient = new WebClient();   final HtmlPage page =     webClient.getPage("http://htmlunit.sourceforge.net");   assertEquals("Welcome", page.getTitleText());    final String pageAsXml = page.asXml();   assertTrue(pageAsXml.contains(""));    final String pageAsText = page.asText();   assertTrue(pageAsText.contains("Support for HTTP(S)")); }`

À la vue de cet exemple, on comprend les possibilités d'un tel outil : test de disponibilité, test de présence de certains éléments (techniques ou fonctionnels) mais aussi vérification du caractère bien formé d'une page HTML/XML. HTMLUnit pousse la technique jusqu'à simuler un navigateur spécifique. Cette possibilité n'est pas superflue quand on connait le respect très variable de la norme HTML des différents navigateurs du marché.

Le parti pris de HTMLUnit de traiter avec la partie présentation est intéressant car il teste la pile complète de l'application et cela au plus près du résultat visible par l'utilisateur. Cela implique cependant que l'application complète soit démarrée et disponible pour être testée, ce qui peut s'avérer lourd à prendre en charge.\
 Le *framework* offre beaucoup de possibilités de tests mais attention à ne pas se perdre à tester trop d'éléments HTML inutiles. Écrire les tests de pages HTML avec ce type d'outil reste fastidieux. Pour y pallier, il existe des outils de plus haut niveau comme [JWebUnit](http://jwebunit.sourceforge.net/), [WebDriver](http://code.google.com/p/selenium/?redir=1) ou [JSFUnit](http://www.jboss.org/jsfunit) (voir la suite) qui s'appuient justement sur HTMLUnit.

[JSFUnit](http://www.jboss.org/jsfunit) quant à lui est le concept de test de la partie présentation telle que vue par HTMLUnit (vu au paragraphe précédent) appliqué à JSF.\
 Les tests JSFUnit sont - comme tous les tests JUnits – des méthodes d'une classe Java. Dans le cas particulier de JSFUnit, il est nécessaire que cette classe hérite de ServletTestCase afin de lui faire prendre en compte la spécificité Servlet de JSF. Une fois les JARs de JSFUnit intégrés au CLASSPATH, il est possible d'utiliser les classes du *framework* et ainsi de tester les Managed Beans, la navigation de page en page, les composants visuels, les résultats de sortie ainsi que la configuration de l'application. L'exemple suivant du site officiel montre le test de la page de présentation d'une application :

`public class JSFUnitTest extends ServletTestCase {   public static Test suite()   {     return new TestSuite(JSFUnitTest.class);   }    public void testInitialPage() throws IOException   {     // Send an HTTP request for the initial page     JSFSession jsfSession = new JSFSession("/index.faces");      // A JSFClientSession emulates the browser     // and lets you test HTML     JSFClientSession client = jsfSession.getJSFClientSession();      // A JSFServerSession gives you access to JSF     JSFServerSession server = jsfSession.getJSFServerSession();      // Test navigation to initial viewID     assertEquals("/index.jsp", server.getCurrentViewID());      // Assert that the prompt component     // is in the component tree and rendered     UIComponent prompt = server.findComponent("greeting");     assertTrue(prompt.isRendered());      // Test a managed bean     assertEquals("Stan",                  server.getManagedBeanValue("#{foo.text}")                  );   } }`

Afin de pouvoir lancer ces tests, il est nécessaire d'inclure dans le fichier web.xml les lignes suivantes :

` <filter>   <filter-name>     JSFUnitFilter   </filter-name>   <filter-class>     org.jboss.jsfunit.framework.JSFUnitFilter   </filter-class> </filter>  <filter-mapping>   <filter-name>     JSFUnitFilter   </filter-name>   <servlet-name>     ServletTestRunner   </servlet-name> </filter-mapping>  <filter-mapping>   <filter-name>     JSFUnitFilter   </filter-name>   <servlet-name>     ServletRedirector   </servlet-name> </filter-mapping>  <servlet>   <servlet-name>     ServletRedirector   </servlet-name>   <servlet-class>     org.jboss.jsfunit.framework.JSFUnitServletRedirector   </servlet-class> </servlet>  <servlet>   <servlet-name>     ServletTestRunner   </servlet-name>   <servlet-class>     org.apache.cactus.server.runner.ServletTestRunner   </servlet-class> </servlet>  <servlet-mapping>   <servlet-name>     ServletRedirector   </servlet-name>   <url-pattern>     ServletRedirector   </url-pattern> </servlet-mapping>  <servlet-mapping>   <servlet-name>     ServletTestRunner   </servlet-name>   <url-pattern>     ServletTestRunner   </url-pattern> </servlet-mapping>`

Une fois l'application déployée, il est possible de lancer les test via une page de celle-ci. Le site officiel donne un exemple en utilisant un rapport [Cactus](http://jakarta.apache.org/cactus/integration/integration_browser.html) :

[![servlettestrunner\_html](http://08000linux.com/blogs/files/2010/09/servlettestrunner_html.jpeg)](http://08000linux.com/blogs/files/2010/09/servlettestrunner_html.jpeg)

De même que HTMLUnit, l'avantage de ce genre de *framework* est de tester et d'utiliser l'application de bout en bout. On ne teste plus de petites parties mais bien l'application dans sa totalité. Cette aspect a les défauts de ses qualités : il est nécessaire de conserver une application complète démarrée pour pouvoir effectuer les tests.

Les possibilités de tests sont de plus nombreuses. Ce qui peut paraître ici comme un avantage peut aussi constituer un inconvénient au développeur : il peut se retrouver perdu au milieu de pages Jsp qu'il serait tenté de tester dans leur intégralité. En cas d'adoption d'un tel outil, il est donc nécessaire de borner les tests à certains éléments précis, au risque de voir les développeurs se perdre dans des lignes de code de tests inutiles.

JSFUnit étant une extension de HTMLUnit pour JSF, il sera plus adapté pour tester les applications JSF.

*Article publié sur [LinuxFr](http://linuxfr.org/~galaux) dans le cadre de mon activité professionnelle à [Linagora](http://linagora.com/).*

