# Integracao sistema cashback
Solução de integração de dados para o sistema de cashback

## Requisitos funcionais
	
* Importação e sincronização de informações de Vendedoras e Produtos para base de dados DocumentDB sistema gerenciador de cashback
    * Implantar uma rotina de busca,transformação e migração de dados para sincronização da dados de Vendedoras.
    * Implantar uma rotina de busca,transformação e migração de dados para sincronização da dados de Produtos.
	
* Salvar as solicitações de cashback no backend do Sistema 
    * Entendo como melhor solução ficar a cargo da aplicação de controle de cashback centralizar os acessos a sua base de dados.

* Enviar para um sistema do Grupo Boticário que está On Premises as informações de 
    * Entendo como melhor solução ficar a cargo da aplicação de controle de cashback fazer uma postagem de mensagem para a integração com o sistema legado.

## Requisitos não funcionais
* **Politicas de retentativa:** Poder configurar regras de retentativa em caso de erro nas postagens no messageBus
* **Resiliência:** Possibilitar uso de um orquestrador containeres, como o Kubernetes
* **Fluxo assíncrono:** Possibilitar entre o fonte e o destino possibilitando uma liberdade maior para mudanças
* **Gestão de logs:** 
    * Gerar logs a cada registro/mensagem comsumida
    * Usar uma ferramenta de centralização de logs
    * Criar rotinas ou configurar TTL para expurgo dos logs
* **Monitoria:** Ter um board que possibilite visualizar o status das integrações de forma simplificada
		Gerar notificações em caso de problemas identificados

## Sugestão de ferramentas para a solução	
   * **Integração, transformação e migração de dados**
       * Apache Nifi
         * Documentação oficial: https://nifi.apache.org/docs/nifi-docs/
         * Exemplo de uso: https://gist.github.com/ijokarumawak/42c257afb5e80361e502564085d7999e
       * Pentaho data integration
         * Documentação oficial: https://help.pentaho.com/Documentation/8.2
   * **MessageBus**
       * RabbitMQ em caso de trabalhar com filas
         * Documentação oficial: https://www.rabbitmq.com/documentation.html
       * Apache kafka em caso de trabalhar com topicos
            * Documentação oficial: https://kafka.apache.org/documentation/
   * **Gestão de Logs**:
       * Stack ELK - https://www.elastic.co/pt/what-is/elk-stack
       * Elasticsearch: Banco de dados de logs
       * Logstach: Formatação de logs
       * Kibana: Visualização de logs       
   * **Monitoria**:
       * APM: Visualização facilitada pra trace e métricas das transações
       * Grafana: Dashboard para monitoria de logs e notificações
         * Documentação oficial: https://grafana.com/docs/

## Desenho da solução de integração	
![solucao-cashback](https://user-images.githubusercontent.com/10811002/88494380-42203700-cf8c-11ea-92da-36790ff4b411.png)

## Exemplo pipeline do apache Nifi
![apache fini process example](https://user-images.githubusercontent.com/10811002/88494601-32552280-cf8d-11ea-85e7-61eea0fc81c4.png)

### Outras possíveis soluções
   * Usar a ferramenta DMS da própria AWS para a migração de dados
porém ficaríamos presos a ferramenta e também encontrei
alguns problemas para manter os dados sincronizados.
   * Usar uma ferramenta statefull que possa gerenciar os steps 2 e 3, como um ESB ou um BPM, nesse escopo existes vários produtos no mercado, mas deve-se tomar cuidado pois normalmente são ferramentas pesadas que podem dificultar o fluxo de atualização/deploy.
