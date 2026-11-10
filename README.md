# RJ3DART Expedição — Android

Aplicativo Android nativo, minimalista, para conferência visual de anúncios durante a separação e embalagem de pedidos.

## Recursos incluídos
- Busca instantânea por palavras ou partes do título, sem diferenciar maiúsculas ou acentos.
- Busca também por SKU e ID, quando informados.
- Lista de produtos com miniaturas e contador de resultados.
- Visualizador interno de fotos com botões anterior/próxima.
- Botão «Conferido» que volta à busca (não grava conferência nem altera pedido).
- Catálogo local persistido no aparelho (busca funciona sem internet).
- Cache temporário de fotos que já foram abertas (o Android pode apagar o cache).
- Importação de JSON ou CSV pelo seletor de arquivos do Android.
- Atualização manual de JSON em uma URL HTTPS.
- Catálogo demonstrativo com 2 anúncios reais da loja RJ3DART e URLs de imagens públicas da Shopee, conferidos em 09/11/2026. Fotos dependem da disponibilidade da Shopee.

## Como compilar
1. Instale Android Studio recente com JDK 17 e SDK Android 35.
2. Abra a pasta `RJ3DART_Expedicao` como projeto.
3. Aguarde a sincronização do Gradle.
4. Selecione **Build > Build APK(s)**.
5. O APK deve ser produzido em `app/build/outputs/apk/debug/app-debug.apk`.
6. Instale no Android (Android 7+). O projeto usa somente APIs nativas do Android, sem bibliotecas externas de interface.

**Importante:** neste ambiente não há Android SDK/Gradle instalados, portanto o projeto não foi compilado nem testado em um dispositivo. O ZIP entregue contém o código-fonte completo, não um APK pronto.

## JSON de catálogo (formato recomendado)
```json
{
  "products": [
    {
      "id": "123456",
      "title": "Porta-incenso decorativo",
      "sku": "",
      "url": "https://shopee.com.br/product/465750017/123456",
      "images": [
        "https://exemplo.com/foto-frente.jpg",
        "https://exemplo.com/foto-lado.jpg"
      ]
    }
  ]
}
```
`id`, `title` e `images` são os campos principais. Se houver variações que precisam ser diferenciadas na expedição, cadastre-as como itens separados no catálogo com título e imagem específicos. Arquivos importados substituem o catálogo anterior.

## CSV suportado
Cabeçalhos: `titulo;id;sku;imagens;url` ou `title,id,sku,images,url`. Use `|` entre URLs das fotos no mesmo campo. Cabeçalhos alternativos: `nome do produto`, `nome do anuncio`, `fotos`, `image`. Para evitar duplicação, linhas com o mesmo ID são agrupadas.

### Exemplo
```csv
titulo;id;sku;imagens;url
Filtro de Ralo Ajustável;58254834047;;https://down-br.img.susercontent.com/file/br-11134207-81ztc-mkf9dptk1gxs7c;https://shopee.com.br/product/465750017/58254834047
```

## Integração com Shopee
O app **não possui conexão autenticada** com a conta da Shopee nem sincroniza automaticamente os anúncios do vendedor. A integração correta pode ser implementada em backend seguro usando a Shopee Open Platform v2, endpoints `/api/v2/product/get_item_list` e `/api/v2/product/get_item_base_info`, com autorização da loja. As chaves e tokens devem ficar no servidor, jamais embutidos no APK. O backend entrega o JSON simplificado acima por HTTPS ao aplicativo.

## Limitações
- As imagens remotas podem exigir internet inicialmente; disponibilidade de URLs públicas pode mudar.
- Cache de fotos é descartável, não um modo offline garantido.
- O app não emite etiquetas, baixa estoque ou marca pedidos como expedidos.
- CSV de exportação da Shopee pode exigir conversão dos cabeçalhos/colunas de fotos para o formato descrito.

## Gerar APK automaticamente no GitHub (sem Android Studio)
1. Crie um repositório GitHub e envie **o conteúdo da pasta** RJ3DART_Expedicao (incluindo `.github/workflows/build-apk.yml`).
2. Acesse a aba **Actions** do repositório, selecione **Gerar APK RJ3DART** e clique em **Run workflow**.
3. Ao concluir com sucesso, baixe o artefato **RJ3DART-Expedicao-APK** e extraia `app-debug.apk` para instalar no celular.
4. Para instalar fora da Play Store, autorize a instalação de aplicativos dessa origem nas configurações do Android.

O APK criado por esse fluxo é uma versão de depuração para testes. Nenhuma compilação foi executada neste ambiente.
