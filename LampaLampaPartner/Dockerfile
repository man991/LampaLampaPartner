FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY LampaLampaPartner.csproj .
RUN dotnet restore LampaLampaPartner.csproj
COPY . .
RUN dotnet build LampaLampaPartner.csproj -c Release -o /app/build

FROM build AS publish
RUN dotnet publish LampaLampaPartner.csproj -c Release -o /app/publish

FROM nginx:alpine AS final
WORKDIR /usr/share/nginx/html
COPY --from=publish /app/publish/wwwroot .
COPY nginx.conf /etc/nginx/nginx.conf

# Copy the start script  
COPY start.sh ./  
# This command is optional,   
# it is just used to make sure all the EOL unix compliant  
RUN sed -i -e "s/\r$//" start.sh  
# The command that will be executed when the container starts  
CMD /bin/sh ./start.sh 