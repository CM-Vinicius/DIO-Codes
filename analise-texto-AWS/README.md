# Análise de Texto com AWS Textract

Este é um código Python que se conecta ao serviço **AWS Textract** para realizar a análise de texto em imagens. O código extrai o texto de um arquivo de imagem e grava a resposta em um arquivo JSON para posterior processamento.

## 🚀 Objetivo

O objetivo deste projeto é utilizar o **AWS Textract** para extrair texto de uma imagem, como um documento, lista ou qualquer outro material, usando a API do Textract.

## 💻 Requisitos

Antes de executar o código, você precisa garantir que o seguinte esteja configurado no seu ambiente:

### Dependências:
- **boto3**: Biblioteca AWS para Python.
- **mypy_boto3_textract**: Tipagens para Textract, para facilitar o trabalho com a API.
- **botocore**: Biblioteca essencial para interagir com os serviços da AWS.

Instale as dependências com o comando:

```
pip install boto3 mypy-boto3-textract

```

## 🔑 Configuração da AWS

Certifique-se de ter suas credenciais da AWS configuradas corretamente para que o código possa se conectar ao AWS Textract. Se ainda não configurou as credenciais, execute o seguinte comando:

```
aws configure

```
Isso pedirá o AWS Access Key ID, AWS Secret Access Key, a região e o formato de saída.


## 🚧 Como Usar

1. Clone o repositório ou baixe o código para o seu computador.
2. Execute o código Python para realizar a análise de texto com o AWS Textract.

Exemplo de código:

```
import json
from pathlib import Path

import boto3
from botocore.exceptions import ClientError
from mypy_boto3_textract.type_defs import DetectDocumentTextResponseTypeDef

def detectar_arquivo() -> None:
    client = boto3.client("textract")

    caminho_imagem = str(Path(__file__).parent / "imagens" / "lista-material-escolar.jpeg")
    with open(caminho_imagem, "rb" ) as f:
        bytes_imagem = f.read()

    try:
        response = client.detect_document_text(Document={"Bytes": bytes_imagem})
        with open("response.json", "w") as resultado:
            resultado.write(json.dumps(response))
    except ClientError as erro:
        print(f"Erro durante processamento de imagem!: {erro}")


def extrair_linhas() -> list[str]:
    try:
        with open("response.json", "r") as f:
            dados: DetectDocumentTextResponseTypeDef = json.loads(f.read())
            blocos = dados["Blocks"]
        return [bloco["Text"] for bloco in blocos if bloco["BlockType"] == "LINE"]
    except IOError:
        detectar_arquivo()
    return[]


if __name__ == "__main__":
    for linha in extrair_linhas():
        print(linha)

```

## ⚡ Funcionamento

- **Detectar Arquivo:** O código usa o AWS Textract para analisar a imagem lista-material-escolar.jpeg (ou qualquer outro arquivo na pasta imagens/). O texto extraído é salvo em response.json.
- **Extrair Linhas:** O código lê o arquivo JSON gerado e extrai as linhas de texto.
- **Processamento:** O texto extraído é impresso no terminal, uma linha por vez.

## 🌐 Contribuições

Sinta-se à vontade para contribuir com melhorias ou correções! Se você encontrar algum bug ou quiser sugerir uma melhoria, abra uma issue ou crie uma **pull request**.













