---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Obtendo seu CDN para Status em Execução
{: #getting-your-cdn-to-running-status}

Saiba como colocar seu CDN em um estado RUNNING seguindo estas diretrizes. Este documento também
informa como iniciar e parar o CDN.

## Obter para Em Execução

Depois de ter criado um CDN, ele aparecerá no painel CDN. Aqui você verá o nome do CDN, a Origem, o Provedor e o status.  

 ![Mapeando a captura de tela de lista](images/mapping-list.png)


Se você solicitou seu CDN com HTTP ou HTTPS com o certificado curinga, será possível continuar
com a Etapa 1.

Se você criou um CDN com HTTPS com o certificado SAN DV, etapas adicionais podem ser necessárias
para verificar seu domínio e podem ser localizadas na página
[Concluindo Domain Control Validation para HTTPS](/docs/infrastructure/CDN/how-to-https.html#completing-domain-control-validation-for-https).

**Etapa 1:**

Depois de ter pedido um CDN, você precisará configurar o **CNAME** com seu provedor DNS. A maioria dos provedores DNS pode fornecer instruções sobre como configurar ou mudar o CNAME.

   * Durante esse tempo, o status do seu CDN será mostrado como **Configuração do CNAME**. Verifique com seu provedor DNS quando as mudanças se tornarão ativas.

   ![Configuração do CNAME](images/cname-config.png)  

**Etapa 2:**

A qualquer momento, depois de ter configurado o CNAME com seu provedor DNS, será possível verificar o status selecionando **Obter status** no menu overflow à direita do status do CDN.

  ![getStatus do CNAME](images/cname-getstatus.png)  

**Etapa 3:**

Quando o encadeamento CNAME estiver concluído, a seleção de **Obter status**
mudará o status para *RUNNING* e o CDN estará pronto para uso.

Parabéns! Agora seu CDN está em execução. A partir de agora, a página
[Gerenciar seu CDN](/docs/infrastructure/CDN/how-to.html#manage-your-cdn) tem informações adicionais sobre como
configurar opções, como [Tempo de
vida](/docs/infrastructure/CDN/how-to.html#setting-content-caching-time-using-time-to-live-), [Limpando conteúdo em cache](/docs/infrastructure/CDN/how-to.html#purging-cached-content) e
[Incluindo detalhes do Caminho de Origem](/docs/infrastructure/CDN/how-to.html#adding-origin-path-details).

## Iniciando o CDN

O início do CDN informa ao DNS para direcionar tráfego de sua origem para o servidor de borda do Akamai. Depois que o mapeamento é iniciado, o cache do DNS ainda pode direcionar o tráfego para a origem para que a funcionalidade não possa ser vista pelo domínio imediatamente após o início do mapeamento. O tempo gasto para atualização depende da frequência com que o cache do DNS é atualizado e varia, dependendo de seu provedor DNS.

**NOTA**: um CDN pode ser iniciado somente quando no status `Stopped`  

**Etapa 1:**

Clique em **Iniciar CDN** no menu Overflow, que aparece como três pontos no lado direito da linha do CDN.

  ![Overflow menu](images/start_cdn.png)

**Etapa 2:**

Aparece uma janela de diálogo maior pedindo para confirmar que você deseja iniciar o serviço. Selecione **Confirmar** para continuar.

**Etapa 3:**

Se a ação tiver sido bem-sucedida, aparecerá uma caixa de diálogo no canto superior direito de sua tela, permitindo que você saiba que ela foi bem-sucedida, junto ao horário.

**Etapa 4:**

Essa etapa muda o Status para `CNAME Configuration`

**Etapa 5:**

Clique em **Obter status** no menu Overflow. Essa etapa muda o status para `Running`. Seu CDN se torna operacional.

## Parando o CDN

Depois que um mapeamento é interrompido, a consulta de DNS é alternada para a origem. O tráfego ignora os servidores de borda do CDN e o conteúdo é buscado diretamente da origem. Depois que um mapeamento for interrompido, poderá haver um breve período de tempo em que seu conteúdo não estará acessível. Isso é porque o cache do DNS ainda pode estar direcionando o tráfego para os servidores de borda do Akamai. No entanto, durante esse tempo, o servidor de borda do Akamai negará tráfego para o domínio. O tempo de duração esse período depende da frequência com que o cache de DNS é atualizado e varia dependendo de seu provedor DNS.

**NOTAS**: 
* **NÃO** é recomendado parar um CDN configurado com um Certificado SAN HTTPS, porque o tráfego HTTPS pode não funcionar quando você mover o CDN de volta para o status `Running`. 
* Um CDN pode ser interrompido somente quando no estado `Running`.

**Etapa 1:**

Clique em 'Parar CDN' no menu Overflow (3 pontos verticais à direita do status CDN).
![Menu Overflow](images/stop_cdn.png)

**Etapa 2:**

Aparece uma janela de diálogo maior, solicitando que você confirme se deseja parar o serviço. Selecione **Confirmar** para continuar.

**Etapa 3:**

Depois de aproximadamente 5 a 15 segundos, o status deve mudar para 'Interrompido'

## Excluindo o CDN

Para excluir um CDN, siga estas etapas:

**NOTA**: selecionar `Excluir` no menu overflow exclui apenas o CDN; ele não exclui sua
conta.

**Etapa 1:**

Clique em 'Excluir' no menu overflow.

 ![Excluir CDN no Menu Overflow](images/delete_cdn.png)

**Etapa 2:**

Aparece uma janela de diálogo maior pedindo para confirmar que você deseja excluir. Clique em **Excluir**
para continuar.

**NOTA**: se seu CDN estiver configurado usando HTTPS com o Certificado SAN DV, poderá
levar até 5 horas para concluir o processo de exclusão.

  ![Delete with Warning](images/delete-with-warning.png)

**Etapa 3:**

Depois de concluir as etapas 1 e 2, o status de seu CDN será `Deleting`. Quando o processo de exclusão estiver concluído, clique em 'Obter status' no menu overflow novamente para remover a linha da lista do CDN. Se o processo de exclusão não tiver sido concluído, esta ação não terá efeito.
