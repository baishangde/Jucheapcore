#Depending on the operating system of the host machines(s) that will build or run the containers, the image specified in the FROM statement may need to be changed.
#For more information, please see https://aka.ms/containercompat

FROM microsoft/aspnetcore:2.0-nanoserver-1803 AS base
WORKDIR /app
EXPOSE 80

FROM microsoft/aspnetcore-build:2.0-nanoserver-1803 AS build
WORKDIR /src
COPY ["JuCheap.Core.Web/JuCheap.Core.Web.csproj", "JuCheap.Core.Web/"]
COPY ["JuCheap.Core.Services/JuCheap.Core.Services.csproj", "JuCheap.Core.Services/"]
COPY ["JuCheap.Core.Data/JuCheap.Core.Data.csproj", "JuCheap.Core.Data/"]
COPY ["JuCheap.Core.Infrastructure/JuCheap.Core.Infrastructure.csproj", "JuCheap.Core.Infrastructure/"]
COPY ["JuCheap.Core.Models/JuCheap.Core.Models.csproj", "JuCheap.Core.Models/"]
COPY ["JuCheap.Core.Interfaces/JuCheap.Core.Interfaces.csproj", "JuCheap.Core.Interfaces/"]
RUN dotnet restore "JuCheap.Core.Web/JuCheap.Core.Web.csproj"
COPY . .
WORKDIR "/src/JuCheap.Core.Web"
RUN dotnet build "JuCheap.Core.Web.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "JuCheap.Core.Web.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "JuCheap.Core.Web.dll"]