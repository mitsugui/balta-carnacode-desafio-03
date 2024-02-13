![banner-carnacode](https://github.com/balta-io/carnacode-balta-2024-desafio-01/assets/965305/b8cc442c-d64f-4dd1-9414-7fc896b47183)

# CarnaCode 2024 - Desafio 3
O CarnaCode é um evento online e gratuito que acontece nos dias 10, 11, 12 e 13 de Fevereiro, onde você terá a oportunidade de codificar uma aplicação Web (Blazor + PWA) completa, do começo ao fim.


👉 https://go.balta.io/carnacode-2024

## Sobre o Desafio
Neste terceiro desafio, adicionamos suporte a PWA e publicamos a aplicação completa para calcular o IMC (Índice de Massa Muscular) que fizemos nos desafios anteriores. Aprendemos conceitos como Git, GitHub, CI/CD, DevOps e PWA.

## Desafios

Fácil: 
	* Publicar a aplicação localmente.

Médio: 
	* Publicar a aplicação em um host qualquer.

Avançado: 
	* Configurar CI/CD para publicar a aplicação automaticamente usando GitHub actions para o GitHub pages.

### Tecnologias Utilizadas
* ASP.NET
* Razor
* Blazor
* PWA
* Git
* GitHub
* CI/CD
* GitHub Actions
* GitHub Pages

### Ferramentas Utilizadas
* Visual Studio

# Recompensa
<img src="https://baltaio.blob.core.windows.net/temp/carnacode-badge-desafio-03.png" alt="CarnaCode 2024 - Terceiro Desafio Completo" width="256" />

 ###  Grupo 01
* [Eliane Henriqueta](https://github.com/Elianehenri)
* [Pablo Alessandre](https://github.com/pabloalessandre)
* [Gabriele Felice](https://github.com/gabi-felice-dev)
* [Julio](https://github.com/mitsugui)


# Passo a passo
1. Copiar arquivos do desafio 2 na pasta clonada (sem .git)
2. Gerar o ícone da aplicação:
	1. Instalar a extensão PWABuilder.
	2. Abrir Icon Generator
	3. Escolher o ícone que está no Assests
	4. Ajustar cor de fundo #2E1462 rgb(46, 20, 98)
	5. Padding de 0
	6. Criar pasta wwwroot/imgs
	7. Colocar ícones na pasta
3. Editar o manifesto
	1. Ajustar os ícones:
```JSON
[
    {
        "src": "imgs/manifest-icon-512.maskable.png",
        "type": "image/png",
        "sizes": "512x512"
    },
    {
        "src": "imgs/manifest-icon-192.maskable.png",
        "type": "image/png",
        "sizes": "192x192"
    },
    {
        "src": "imgs/apple-icon-180.png",
        "type": "image/png",
        "sizes": "180x180"
    }
]
````
4. Fazer publish para pasta:
    1. No Visual Studio basta clicar com botão direito no projeto e escolher Publish...
    2. Escolher a opção de publicação para uma pasta.
    3. Publicar a aplicação.
5. Executar um servidor web
    1. ```npm install -g http-server```
    2. Navegar até a pasta wwwroot que está dentro da pasta publicada (procurar o último nível)
    3. Executar ```http-server -c1```
    4. Abrir no browser.

7. Publicar no GitHub Pages. [Referência 1](https://www.youtube.com/watch?v=0cvpumXKhBI)
    1. Criar Token para a automação.
        1. Clicar na foto do perfil e Settings.
        2. Developer settings.
        3. Personal access tokens.
        4. Generate new token (Classic).
            1. Note: GITHUB_TOKEN
            2. Expiração: No expiration
            3. Permissões:
                workflow
                write:packages
                admin:org
                admin:public_key
                admin:repo_hook
                admin:org_hook
                user
                codespace
                project
                admin:gpg_key
                admin:ssh_signing_key
        5. Copiar a chave gerada.
    2. Criar action para preparar o ramo que será usado no github pages.
        1. Criar arquivo na pasta 
        <pasta da solução>\.github\workflows\main.yml

```YAML
        name: Deploy Blazor WASM to GitHub Pages

on:
    push:
        branches: [main]

jobs:
    deploy-to-github-pages:
        runs-on: ubuntu-latest
        steps:
            - uses: actions/checkout@v2

            - name: Setup .NET Core SDK
              uses: actions/setup-dotnet@v1
              with:
                  dotnet-version: 8.0.101
                  include-prerelease: true

            - name: Publish .NET Core Project
              run: dotnet publish IMC/IMC.csproj -c Release -o release --nologo

            - name: Change base-tag in index.html to match GitHub Pages repository subdirectory
              run: sed -i 's/<base href="\/" \/>/<base href="\/balta-carnacode-desafio-03\/" \/>/g' release/wwwroot/index.html

            - name: Add .nojekyll file
              run: touch release/wwwroot/.nojekyll

            - name: Commit wwwroot to GitHub Pages
              uses: JamesIves/github-pages-deploy-action@3.7.1
              with:
                  GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
                  BRANCH: gh-pages
                  FOLDER: release/wwwroot

```
        2. Substituir o texto "balta-carnacode-desafio-03" na linha ```run: sed...``` pelo nome do repositório github.
        3. Fazer commit do arquivo.
    3. Verificar se o workflow foi executado com sucesso.
        1. Acessar a aba "Actions" do github.
        2. Clicar no workflow "Deploy Blazor WASM to GitHub Pages".
    4. Configurar GitHub pages.
        1. Acessar a aba "Settings" do repositório.
        2. Descer até a seção "GitHub Pages".
        3. Ma sessão "Build and deployment":
            1. Escolher Source "Deploy from a branch". 
            2. Escolher a opção "gh-pages" e salvar.
        4. Acessar o link gerado.