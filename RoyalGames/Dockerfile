# 1. Etapa de compilacao do projeto (Build)
#
# Aqui o docker esta chamando a imagem que tem 
# a ferramenta que consegue compilar o codigo
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS Build
WORKDIR /app_build

# Copiando o arquivo .csproj e instalando as dependencias que o projeto usa
#
# Aqui o docker esta instalando todas as dependencias / pacotes
# Que estao precetadas no arquivo de configuracao do projeto ".csproj"
#
# Se fosse no next ou node "package.json"
COPY RoyalGames.csproj ./
RUN dotnet restore

# Copia todas as pastas do projeto (Controllers, Domains, Contexts...)
#
# Nesta etapa o Docker esta copiando as pastas e arquivos
# para compilar tudo e gerar os arquivos finais ja prontos para rodar 
# no C# o ".dll"
COPY . ./
RUN dotnet publish -c Release -o /app_publish

#
#
# 2. ETAPA DE EXECUCAO (RUNTIME)
#
# Aqui o Docker esta preparando o ambiente de execucao
# E gerando a imagem de execucao do projeto
FROM mcr.microsoft.com/dotnet/aspnet:8.0  AS RUNTIME
WORKDIR /app_build
COPY --from=build /app_publish .

# Expoe a porta padrao do container para o ASP.NET Core web API
#
# Avisando para o docker em qual porta a api vai escutar requisicoes
EXPOSE 8080

# Comando para rodar a API usando a DLL do projeto
# Definindo o comando que vai startar a minha aplicacao
ENTRYPOINT ["dotnet", "RoyalGames.dll"]