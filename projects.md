---
layout: default
---

# Projetos

Caso deseje mais informações sobre um projeto específico, por favor, entre em contato. 

### SyncBling <small><i style="font-size: 20px; color: #ED5E00" class="fa-brands fa-magento"></i></small>

<p>
Módulo desenvolvido para Magento 1. Sincroniza o status "Enviado" entre Magento e a plataforma de ERP Bling.
Caso o status do pedido esteja como Enviado no ERP, o status do pedido no Magento é alterado.
A execução ocorre através de um cron, uma vez por dia. Os pedidos sincronizados ficam salvo em um arquivo de log único.
</p>
<a href="https://github.com/fkdarven/magento1_syncbling" class="see-on-github">Visualizar projeto no github <i style="font-size: 25px; color:"  class="fa-brands fa-github"></i></a> 

--------

### Integração Pierserv <small><i style="font-size: 20px; color: #21759B" class="fa-brands fa-wordpress"></i></small>

<p>
Integração sob demanda entre Wordpress e a plataforma de ERP Pierserv. Neste projeto foram criados um cron, um webhook e um plugin.

O plugin consiste nos métodos de envio ofertados pela PierServ, a partir do CEP fornecido pelo cliente. O cron é responsável por sincronizar o estoque do Woocommerce de acordo com o inventário da PierServ, e o webhook 
envia os pedidos a Pier toda vez que o status de um pedido muda para "Processando" (pagamento efetuado).
</p>
<a href="mailto:contato@darven.wtf" class="private-project">Projeto Privado <i class="fa-regular fa-file-lock"></i> </a>

-------

### Plugin de Múltiplos Preços Informativos <small><i style="font-size: 20px; color: #21759B" class="fa-brands fa-wordpress"></i></small>

Plugin desenvolvido para Woocommerce visando mostrar mais de um preço na página do produto/categorias. Como, por exemplo, preço á vista e preço parcelado. Essa demanda surgiu
visto que, os plugins já existentes estavam gerando erro em produtos agrupados (pacote de produtos), mostrando o preço como NaN.

<span class="executing">Projeto em Execução</span>

-------

### Melhorias Pagespeed <small><i style="font-size: 20px; color: #ED5E00" class="fa-brands fa-magento"></i></small>

Aplicação de múltiplas técnicas para melhorar o desempenho de uma loja Magento 1 no Google PageSpeed. Entre as metodologias estão:

- Otimização de Imagens (O Magento 1 não permite formatos modernos como .webp e .avif)
- Otimização dos arquivos Javascript (Magento 1 carrega as mesmas bibliotecas múltiplas vezes)
- Reestruturação do Owl Carousel, para que ele não seja mais o último elemento a carregar na página
- Aumento da Nota de 65 para 91

<!-- <a href="https://github.com/fkdarven/magento1_optimize" class="see-on-github">Visualizar projeto no github <i style="font-size: 25px; color:"  class="fa-brands fa-github"></i></a> !-->
-------
### Integração Woocommerce e Data Studio <small><i style="font-size: 20px; color: #21759B" class="fa-brands fa-wordpress"></i> <i style="font-size: 20px; color: #4285F4" class="fa-brands fa-google"></i></small>

Desenvolvimento de uma Dashboard no Looker Studio (antigo Data Studio), a partir de dados abstraidos de uma loja Woocommerce. Os dados são filtrados com um script PHP e gravados num banco de dados definido no momento da execução.
A partir disso, a Dashboard pode ser conectada no banco de dados e as informações utilizadas.

<a href="https://github.com/fkdarven/woo-google" class="see-on-github">Visualizar projeto no github <i style="font-size: 25px; color:"  class="fa-brands fa-github"></i></a> 
--------



[Voltar](./)
    