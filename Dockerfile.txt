# Fase 1: Build dell'applicazione
FROM ://microsoft.com AS build-env
WORKDIR /app

# Copia i file del progetto e ripristina le dipendenze (NuGet)
COPY *.csproj ./
RUN dotnet restore

# Copia tutto il resto e compila in modalità Release
COPY . ./
RUN dotnet publish -c Release -o out

# Fase 2: Creazione dell'immagine finale leggera per l'esecuzione
FROM ://microsoft.com
WORKDIR /app
COPY --from=build-env /app/out .

# Porta su cui la Web API ascolterà
EXPOSE 8080

ENTRYPOINT ["dotnet", "MiaAppCsharp.dll"]
