FROM mcr.microsoft.com/dotnet/sdk:latest AS builder 
#EXPOSE 8080
#EXPOSE 8443
WORKDIR /Application

#ENV ASPNETCORE_ENVIRONMENT=Development

# Обновляем CA сертификаты
RUN apt-get update && apt-get install -y ca-certificates && update-ca-certificates

# Копируем только файлы проектов для восстановления пакетов
#COPY ["webapi.csproj", "nuget.config", "./"]
COPY ["webapi.csproj", "./"]

# Восстанавливаем пакеты из nuget-packages (если есть) и официальных источников
#COPY nuget-packages/ /nuget-packages/
RUN dotnet restore "webapi.csproj" 
#--source /nuget-packages --source https://api.nuget.org/v3/index.json

# Копируем остальные файлы проекта
COPY . ./

# Публикуем приложение
RUN dotnet publish -c Release -o output

FROM mcr.microsoft.com/dotnet/aspnet:latest
WORKDIR /Application
COPY --from=builder /Application/output .
ENTRYPOINT ["dotnet", "webapi.dll"]