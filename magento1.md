---
layout: default
---


# Problemas com a migração para OpenMage

Abaixo listo algum dos problemas com módulos ou funcionalidades que encontrei em atualizações de Magento 1 para OpenmMage, assim como possíveis
mitigações.



<details>
<summary>Query Commerce</summary>
<div class="container">
  <div class="row">
    <div class="col-xs-12">
      <div class="table-responsive" data-pattern="priority-columns">
        <table  class="table table-bordered table-hover">
          <tbody>
            <tr>
              <td><i class="fa-solid fa-triangle-exclamation"></i> <b>Tipo de Problema</b></td>
              <td>Módulo</td>
            </tr>
            <tr>
              <td>Status</td>
              <td>Completo</td>
            </tr>
          </tbody>
          <tfoot>
            <tr>
              <td colspan="5" class="text-center">Data retrieved from <a href="http://www.infoplease.com/ipa/A0855611.html" target="_blank">infoplease</a> and <a href="http://www.worldometers.info/world-population/population-by-country/" target="_blank">worldometers</a>.</td>
            </tr>
          </tfoot>
        </table>
      </div><!--end of .table-responsive-->
    </div>
  </div>
</div>
<br>
<h2>Introdução</h2>
<p>
A Query Commerce é uma empresa que oferece várias soluções de módulos para Magento 1 e 2. Como métodos de pagamento (Cielo) e entrega (Correios),
<i>feed</i> de produtos para Google Shopping, <i>feed</i> para Facebook e etc.
</p>
<br>

<h2>Problemas e Mitigações</h2>
<br>
<h3>IonCube</h3>
<p>
Todos os módulos para Magento 1 da Query são encriptografados pelo IonCube, sendo este um módulo para PHP para proteger acessos diretos ao código fonte, prevenindo por exemplo ataques de <i>malware</i>.
<br>

　　O problema surge uma vez que, o Magento 1 roda com PHP 5.6, enquanto que o OpenMage roda na versão 7.2 (mínimo). Com isso, os módulos da query
são criptografados usando o IonCube do PHP 5.6, e, consequentemente, ao usar o PHP 7.2+, se tornam incompátiveis. Além dos módulos não
funcionarem, também geram erros fatais na maioria das páginas da loja.
</p>
<br>
<h4>Mitigações</h4>
<p>
A solução mais rápida é a remoção de todos os módulos da Query, dessa forma, não haverá problemas com a criptografia do IonCube. Contudo, essa
não é a melhor alternativa em cenários onde o cliente continuará a utilizar os módulos da Query.
<br>
　　Para tal, a mitigação mais adequada é solicitar diretamente a Query a versão do módulo compilada para o IonCube do PHP 7. Para tal, é
necessário que quem requira seja o cliente que comprou o módulo. Também deve-se atentar para o fato de que a Query usualmente fornece
a licença apenas para a URL produtiva, e, portanto, não funcionará no ambiente de homologação/desenvolvimento. 
</p>
<p>
<b>OS*:</b> Caso o cliente possua a loja hospedada pela Query, e compre o código fonte da loja, os módulos virão sem a criptografia. O que
resolve de ante-mão os problemas da mudança de versão do PHP.</p>
<br>
</details>

<details>
<summary>Enhanced Admin (BL CustomGrid)</summary>
<div class="container">
  <div class="row">
    <div class="col-xs-12">
      <div class="table-responsive" data-pattern="priority-columns">
        <table summary="Informações sobre o módulo enhanced admin' functionality" class="table table-bordered table-hover">
          <caption class="text-center"> </caption>
          <tbody>
            <tr>
              <td><i class="fa-solid fa-triangle-exclamation"></i> <b>Tipo de Problema</b></td>
              <td>Módulo</td>
            </tr>
            <tr>
              <td>Estado</td>
              <td>Trabalho em Progresso</td>
            </tr>
            <tr>
              <td>Solucionado?</td>
              <td>Parcialmente</td>
            </tr>
          </tbody>
          <tfoot>
          </tfoot>
        </table>
      </div><!--end of .table-responsive-->
    </div>
  </div>
</div>

<h2>Introdução</h2>
<p>
O módulo Enhanced Admin, também conhecido como BL CustomGrid, tem como objetivo customizar as colunas existentes no painel administrativo do Magento 1, como em produtos, pedidos, clientes ou 
em outros menus da administração. <br>　　Exemplos de colunas extras que podem ser adicionadas: Forma de Pagamento, Método de Entrega, Status de pedido colorido,
Valor com desconto e entre outros. Além disso, também permite ao usuário exportar os dados para xml ou csv.
</p><br>
<h2>Problemas e Mitigações</h2>
<br>
<h3>Banco de Dados</h3>
<p>
Os módulos do Magento 1 muitas vezes possuem arquivos <i>mysql-upgrade</i>, que permitem mudanças a colunas e tabelas no banco de dados, 
sem afetar o funcionamento do módulo. Dessa forma, quando o módulo é instalado, ele procura pela última versão do <i>mysql-upgrade</i> para ajustar
as tabelas necessárias.
<br>　　
Contudo, com a migração para OpenMage, esses arquivos muitas vezes não são executados, e, dessa forma, as tabelas, colunas e campos que deveriam
ser atualizados, não são. Sendo essas:
<br><br>
- customgrid_grid_column.is_missing <br>
- customgrid_grid_column.id <br>
- customgrid_grid_column.is_edit_allowed <br>
- customgrid_grid_column.customization_params <br>
- customgrid_grid_role.available_profiles <br>
</p>
<br><br>
<h4>Mitigações</h4>
<p>
Esse problema pode ser solucionado de <b>três</b> maneiras diferentes. A primeira solução, é forçar a última versão do arquivo
<i>mysql-upgrade</i> a ser executar, fazendo com que o banco de dados seja atualizado.
<br>
　　Caminho padrão para o arquivo:
<br>
- app/code/community/BL/CustomGrid/sql/customgrid_setup
<br><br>
　　O segundo método é modificar as tabelas diretamente no banco de dados. Caso escolha esse método, é aconselhável fazer um backup do banco antes. É só rodar
os seguintes comandos:
</p>
<pre style="color: blue">
<code>
ALTER TABLE `customgrid_grid_column` CHANGE `is_missing`
`missing` TINYINT(1) UNSIGNED NOT NULL DEFAULT '0';
ALTER TABLE `customgrid_grid_column` ADD `id` INT NOT NULL
AFTER `customization_params`;
ALTER TABLE `customgrid_grid_column` CHANGE
`is_edit_allowed` `allow_edit` TINYINT(1) UNSIGNED NOT NULL
DEFAULT '1';
ALTER TABLE `customgrid_grid_column` CHANGE
`customization_params` `custom_params` TEXT CHARACTER SET utf8
COLLATE utf8_general_ci NULL DEFAULT NULL;
ALTER TABLE `customgrid_grid_role` ADD `available_profiles` INT
NOT NULL AFTER `default_profile_id`;
</code>
</pre>
<small>Comandos a serem executados. Atenção caso seu banco possua algum prefixo, ex: str_, pois será necessário editar o código</small>
<br><br>
<p>
　　A terceira e mais trabalhosa solução, é modificar todas as referências diretamente no código, para que elas passem a ter a nomenclatura da 1ª versão 
do módulo.
</p>

<h3>Conflito com o módulo Magestore Bannerslider</h3>
<p>
O módulo BL tenta adicionar colunas extras na página do módulo Magestore Bannerslider (responsável pelo gerenciamento de banners), causando uma
inconsistência na hora que os dados são recuperados do banco de dados. Dessa forma, ocorre um erro no carregamento da página do Bannerslider
e o mo´dulo não funciona.
</p>
<h4> Mitigações </h4>

Baixar essa versão do módulo:
<p>
<a src="https://github.com/blmage/mage-enhanced-admin-grids/tree/1.0.0-wip"> Enhanced Admin Grids </a><br><br>
　　Faça a instalação do módulo normalmente. Após isso, vá em Sistema -> Custom Grids -> Lista. Selecione todas as grades, desabilite-as e
habilite-as novamente.
</p>
</details>

<details>
<summary>
PayPal Plus
</summary>
<br><br>
<h2>    Introdução</h2>
<p>
O módulo PayPal Plus permite aos usuários integrarem seus websites Magento 1.x ao PayPal, seja com o redirecionamento ao PayPal para pagamento
ou com o <i>checkout</i> transparente, que permite utilizar o modal do PayPal diretamente na sua loja.
</p>
<br>
<h2>Problemas e Mitigações</h2>
<br>
<h3>Uso de colchetes para acessar indexes de arrays </h3>
<p>
　　O módulo do PayPal possui uma função de validação de CPF, onde é armazenado em um <i>array</i> e acessado usando colchetes {}, por exemplo:
</p>

<code>
$piece_of_cpf = $cpf{2};
</code><br>
<small>Exemplo do uso de colchetes para acesso a indexes de <i>arrays</i> no PHP 5</small>
<br><br>

　　Porém, a partir do PHP 7, esse método de acesso não é mais suportado, ocorrendo o seguinte erro:
<br>
<br>
<code>
Fatal error: Array and string offset access syntax with curly braces is no longer supported.
</code>
<br><br>
<p>
　　O PayPal exige que todos os campos sejam validados para o carregamento do modal. Como esse campo com acesso usando colchetes dá erro,
ele não é validado e consequentemente o modal do PayPal não carrega.</p>
<br>
<h3>Mitigações</h3>

A solução é simples, basta substituir os acessos com colchetes por acessos com chaves. {} -> []
<br><br>

<h3>Descontos não são considerados no valor final enviado ao PayPal</h3>
<small>> Esse problema não tenho certeza se é relativo ao PHP 7</small>
<br>
<p>
O PayPal não considera os descontos corretamente, mostrando o valor original dos itens, sem o desconto. O problema aqui é a forma
como o PayPal calcula os descontos, retornando sempre o valor 0, fazendo com que o valor errado seja enviado ao PayPal a cada compra.</p>
<br>
<h4> Mitigações</h4>

<p>Uma possível solução é alterar a função que calcula o desconto, assim como o <i>array</i> de preços enviados ao PayPal. No caso da função,
o laço original buscava descontos apenas no escopo do PayPal, e não da compra (loja) em si. Sendo assim, é só alterar para que o valor
do desconto da compra seja retornado, no arquivo <b>Helper/Data.php</b>, com:</p>
<br>
<pre>
<code>
$quote         = Mage::getModel('checkout/session')->getQuote();
$quoteData     = $quote->getData();
$grandTotal    = $quoteData['grand_total'];
$discountTotal = 0;

foreach ($quote->getAllItems() as $item){
    $discountTotal += $item->getDiscountAmount();
}
return $discountTotal; // ou return $discountTotal *-1;
</code>
</pre>
<small>Fragmento que deve substituir a função padrão </small>
<br><br>
　　Já no <i>array</i> de preços, basta multiplicar a váriave $discount por -1, para que o desconto seja negativo, na linha <b>533</b> do arquivo
<b>Model/Plus/Iframe.php</b>. Você também pode optar por retornar o $discountTotal já negativo, alterando o código acima.
<br>
<p>　　Tenho um repositório no Github que já implementa as soluções para os problemas mencionados aqui: <a href="http://www.github.com/fkdarven/magento-module"> PayPal Plus </a></p>

</details>
    
<details>
<summary>
Bannerslider Magestore
</summary>
<br><br>
<h2>Introdução</h2>

<p>O módulo Bannerslider Magestore possibilita que banners e sliders sejam adicionados a páginas do website. Esas ferramenta é muitas vezes essencial para os comerciantes brasileiros, uma vez que se pode anunciar promoções, dicas, produtos em alta e etc através dos banners.
</p><br>
<h2>Problemas e Mitigações</h2>
<br>
<h3>Data de início e fim não são salvas</h3>

<p>É possível definir a data de ínicio e fim da exibição de um banner ou slider. Porém, a máscara da data no painel exibe no padrão <b>Dia/Mês/Ano</b>, enquanto que na hora de salvar no banco de dados, é salvo como <b>Mês/Dia/Ano</b>. 
<br>
　　Dessa forma, se tentarmos salvar uma data como por
exemplo 29/10/2022, na hora de inserir no banco de dados, constará como 10/29/2022, e portanto, a informação não será salva (mês inexistente).</p>

<br><br>
<h4> Mitigações</h4>
<p>
A solução é simples, sendo necessário apenas converter as váriaveis de data de ínicio e fim, antes delas serem inseridas no banco de dados. Elas são declaradas no arquivo: <br>
<br>

Bannerslider\controllers\Adminhtml\Bannerslider\BannerController 
<br>

As variáveis são <code>$data['start_time']</code> e <code>$data['end_time']</code>, onde, fazendo a conversão, fica:
</p><br>
<pre>
<code>
//Primeiro subsititui os / da data (11/26/2021) para (11-26-2021), logo depois converte
//para o padrão brasileiro, sendo (26-11-2021);

$handler_for_start = str_replace('/', '-', $data['start_time']);
$data['start_time'] = date('d-m-Y', strtotime($handler_for_start));

$handler_for_end = str_replace('/', '-', $data['end_time']);
$data['end_time'] = date('d-m-Y', strtotime($handler_for_end));
</code>
</pre>
<small>Trecho de código para converter a data do formato americano para o brasileiro</small>

<br><br>
<h3>Os banners não saem de exposição na data selecionada</h3>

<p>
O módulo permite definir uma data de ínicio e fim para exibição dos banners, onde, após a data final, o banner para de aparecer no site.
Contudo, mesmo com a definição da data, os banners continuam aparecendo. <br>　　Isso se deve, ao fato de que, não há nenhuma condição no arquivo .phtml
do banners que filtre quais deverão serem exibidos.</p>
<br>

<h4>Mitigações</h4>

<p>Basta que seja criado a condição para mostrar somente banners dentro do intervalo programado. Lembrando que, a função de calculo de data
deve ser implementada por você, de acordo com o seu fuso horário.</p>
<br>
<pre>
<code>
if($today > $banner['end_time']):
	//código do banner
endif;
</code>
</pre><small>Trecho de código para fazer com que banners com data vencida não apareçam mais</small>

</details>
