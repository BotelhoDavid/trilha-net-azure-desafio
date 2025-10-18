# ================================
# Etapa base (runtime)
# ================================
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080

# ================================
# Etapa build
# ================================
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copia apenas o arquivo de projeto para cache
COPY ["trilha-net-azure-desafio.csproj", "./"]
RUN dotnet restore "trilha-net-azure-desafio.csproj"

# Copia todo o restante do código
COPY . .

# Compila e publica
RUN dotnet build "trilha-net-azure-desafio.csproj" -c Release -o /app/build
RUN dotnet publish "trilha-net-azure-desafio.csproj" -c Release -o /app/publish /p:UseAppHost=false

# ================================
# Etapa final (runtime)
# ================================
FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .

# Aplica migrations ao iniciar (EF Core)
ENTRYPOINT ["dotnet", "trilha-net-azure-desafio.dll"]
