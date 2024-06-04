<div align="center">
  <a href="https://github.com/joseferreira-dev/my-study-notes/tree/main/dart"><img src="../../banner.png"></a>
</div>
<br>

# Acessando Dados de APIs

- [Introdução](#introdução)


## Introdução

É muito simples acessar dados de APIs públicas com Dart. Para isso, pode-se utilizar o pacote `http`, que facilita muito o processo de acesso.

## Acessando Dados de APIs com `http`

Para demonstrar como acessar dados de uma API pública, será utilizada a API do ViaCEP, que fornece informações de endereços brasileiros com base no CEP (Código de Endereçamento Postal).

Primeiramente, deve-se adicionar o pacote http. Neste exemplo, será utilizado o pacote em sua versão `1.2.1`.

```shell
dart pub add http
```

No arquivo principal devem ser importados os pacotes necessários e em seguida escrito o código de acesso, conforme se segue:


```dart
import 'dart:convert';
import 'package:http/http.dart' as http;

// Função que acessa os dados para determinado CEP recebido como parâmetro
Future<void> fetchCepData(String cep) async {
  final url = 'https://viacep.com.br/ws/$cep/json/';

  // Tenta fazer o acesso com uma requisição GET
  try {
    final response = await http.get(Uri.parse(url));

    // Se obtiver uma resposta faz a impressão dos dados recebidos
    if (response.statusCode == 200) {
      // Parsear o JSON usando jsonDecode
      final data = jsonDecode(response.body);

      // Manipulando os dados
      print('CEP: ${data['cep']}');
      print('Logradouro: ${data['logradouro']}');
      print('Complemento: ${data['complemento']}');
      print('Bairro: ${data['bairro']}');
      print('Cidade: ${data['localidade']}');
      print('UF: ${data['uf']}');
    } else {
      print('Erro: ${response.statusCode}');
    }
  } catch (e) {
    print('Erro ao buscar dados: $e');
  }
}

void main() async {
  // Adicionando um CEP
  String cep = '50980595'; // CEP válido
  await fetchCepData(cep);
}
```
