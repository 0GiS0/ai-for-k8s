# ⎈ Cómo la IA puede ayudarte con tus clústeres de Kubernetes ⎈

En este repo quiero compartir contigo tres formas en las que la inteligencia artificial puede ayudarte con tus clústers de Kubernetes. Para que puedas probar todo, y no tengas que instalar nada, puedes ejecutar este repo como un Dev Containers o en GitHub Codespaces, [¡del cual tienes 15GB/mes y 120 horas gratuitas al mes en tu cuenta personal!](https://docs.github.com/en/billing/managing-billing-for-github-codespaces/about-billing-for-github-codespaces#monthly-included-storage-and-core-hours-for-personal-accounts)

# Cómo la IA puede ...


<details>
<summary> <h2>🎁 ... ayudarte a aprender 👩🏼‍🏫 Kubernetes</h2></summary>

## GitHub Copilot 

<img src="images/GitHub Copilot logo.png" width="30%" />

Con GitHub Copilot puedes aprender Kubernetes de forma más rápida y sencilla. Puedes utilizar el chat o simplemente escribir código y GitHub Copilot te ayudará a completarlo.

> Puedes probar GitHub Copilot de forma gratuita en [GitHub Copilot Individual Free Trial](https://github.com/github-copilot/signup).

Una vez que tengas la cuenta, puedes usar cualquiera de estos IDEs para probar GitHub Copilot:

En Visual Studio Code solo tienes que instalar la extensión de GitHub Copilot: [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) y ver este vídeo 😊


</details>

<details>
<summary> <h2>🎁 ... trabajar 👷🏼‍♀️ en tus clústeres de Kubernetes</h2></summary>

## kopylot

Se trata de otra herramienta que te ayuda a trabajar con tus clústers de Kubernetes. Puedes instalarlo con:

```bash
pip install kopylot
```

Pero si abres este repo como un Dev container no tienes que instalar nada 😉 Lo que sí que necesitas es exportar la siguiente variables de entorno con tu API key de OpenAI:

```bash
export KOPYLOT_AUTH_TOKEN=<YOUR_OPENAI_API_KEY>
```

Ejemplos de peticiones:

1. *Crea un despliegue con nginx que tenga 5 réplicas y que pueda acceder a él a través del puerto 8080. Este despliegue debe estar dentro del namespace kopylot-demo, el cual también debe ser creado*
2. *Recupera los pods del nginx alojados en el namespace llamado kopylot-demo*

## kubectl ai

kubectl ai es un plugin de kubectl que te ayuda a trabajar con tus clústers de Kubernetes. Puedes instalarlo con:

```bash
brew tap sozercan/kubectl-ai https://github.com/sozercan/kubectl-ai
brew install kubectl-ai
```

Si quieres probarlo con Azure Open AI puedes seguir estos pasos:

### Azure Open AI

```bash
# Variables
RESOURCE_GROUP="ai-loves-k8s"
LOCATION="canadaeast"
AZ_OPEN_AI="aifork8s"

# Login to Azure
az login

# Create Azure Open AI resource
az group create --name $RESOURCE_GROUP --location $LOCATION

# Create Azure Open AI resource
az cognitiveservices account create \
--kind OpenAI\
 --name $AZ_OPEN_AI \
 --custom-domain $AZ_OPEN_AI \
 --sku S0 \
 --resource-group $RESOURCE_GROUP \
 --location $LOCATION

# Create Chat GPT-4 deployment
az cognitiveservices account deployment create \
--name $AZ_OPEN_AI \
--resource-group  $RESOURCE_GROUP \
--deployment-name gpt-4 \
--model-name gpt-4 \
--model-version "0613"  \
--model-format OpenAI \
--sku-capacity "10" \
--sku-name "Standard"
```

Y exportar a las siguientes variables de entorno la información de tu modelo de GPT4:

```bash
export OPENAI_API_KEY=$(az cognitiveservices account keys list --name $AZ_OPEN_AI --resource-group $RESOURCE_GROUP --query key1 -o tsv)
export OPENAI_DEPLOYMENT_NAME=gpt-4
export OPENAI_ENDPOINT=$(az cognitiveservices account show --name $AZ_OPEN_AI --resource-group $RESOURCE_GROUP --query 'properties.endpoint' -o tsv)
```

Si quieres puedes guardar estas variables en un .env
    
cat <<EOF > .env
export OPENAI_API_KEY=$OPENAI_API_KEY
export OPENAI_DEPLOYMENT_NAME=$OPENAI_DEPLOYMENT_NAME
export OPENAI_ENDPOINT=$OPENAI_ENDPOINT
EOF

Y cargarlas con:

```bash
source .env
```

</details>


<details>
<summary> <h2>🎁 ... analizar 🧐 tus clústeres de Kubernetes</h2></summary>

## k8sgpt

<img src="images/k8sgpt-logo.png" width="20%" />

k8sgpt es un modelo de lenguaje de inteligencia artificial que te ayuda a analizar tus clústers de Kubernetes. Puedes instalarlo con:

```bash
brew tap k8sgpt-ai/k8sgpt
brew install k8sgpt
```

Pero si abres este repo en un Dev Container o en GitHub Codespaces ya lo tienes instalado 😉

Lo único que te queda por hacer es registrar tus credenciales en k8sgpt:

```bash
k8sgpt auth add --backend azureopenai \
--baseurl $OPENAI_ENDPOINT \
--engine $OPENAI_DEPLOYMENT_NAME \
--password $OPENAI_API_KEY \
--model gpt-4
```

Y ahora puedes preguntarle a k8sgpt sobre tus clústeres de Kubernetes:

```bash
k8sgpt analyze --namespace something-is-wrong --backend azureopenai
k8sgpt analyze --explain --namespace something-is-wrong --backend azureopenai
```

## kopylot

Esta herramienta te ayuda a identificar problemas en tu clúster. Puedes instalarlo con:

```bash
pip install kopylot
```

Pero si abres este repo como un Dev container no tienes que instalar nada 😉 Lo que sí que necesitas es exportar la siguiente variables de entorno con tu API key de OpenAI:

```bash
export KOPYLOT_AUTH_TOKEN=<YOUR_OPENAI_API_KEY>
```

Para probar esta opción puedes desplegar este manifiesto con algunos problemillas:

```bash
kubectl apply -f wrong-manifests/wrong-resources.yaml
```

Si echas un vistazo al namespace `something-is-wrong` te darás cuenta de que algunas cosas no van bien:

```bash
kubectl get all -n something-is-wrong
kubectl get pods -n something-is-wrong
```

Vamos a preguntarle a kopylot qué está pasando:

```bash
kopylot diagnose deployment nginx
kopylot diagnose pod nginx-786466f74d-bbwg6
```

También puedes utilizar kopylot audit para obtener más información sobre un recurso:

```
kopylot audit pod nginx-786466f74d-bbwg6
```

</details>