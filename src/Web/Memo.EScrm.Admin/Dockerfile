#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["Memo.EScrm/src/Web/Memo.EScrm.Admin/Memo.EScrm.Admin.csproj", "Memo.EScrm/src/Web/Memo.EScrm.Admin/"]
COPY ["Memo.EScrm/src/ApiGateways/Caller/Memo.EScrm.Caller/Memo.EScrm.Caller.csproj", "Memo.EScrm/src/ApiGateways/Caller/Memo.EScrm.Caller/"]
COPY ["Memo.EScrm/src/Contracts/Memo.EScrm.Contracts.Order/Memo.EScrm.Contracts.Order.csproj", "Memo.EScrm/src/Contracts/Memo.EScrm.Contracts.Order/"]
RUN dotnet restore "Memo.EScrm/src/Web/Memo.EScrm.Admin/Memo.EScrm.Admin.csproj"
COPY . .
WORKDIR "/src/Memo.EScrm/src/Web/Memo.EScrm.Admin"
RUN dotnet build "Memo.EScrm.Admin.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Memo.EScrm.Admin.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Memo.EScrm.Admin.dll"]