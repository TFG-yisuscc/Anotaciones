# Primera instalación de OLLAMA 
## Recursos web utilizados 
[Instalación de ollama]()
[ollama como servidor externo](https://github.com/ollama/ollama/blob/main/docs/faq.md#how-do-i-configure-ollama-server)
[ollama como servidor externo II ](https://atlassc.net/2024/10/24/how-to-share-ollama-server-through-ip-address-and-port)
[ Comparación interesante entre ollama y hugging face ](https://datascientistsdiary.com/hugging-face-vs-ollama/)
[Ollama en rpi5](https://itsfoss.com/raspberry-pi-ollama-ai-setup/)
[Calculadora de pesos utilizados](https://llm-system-requirements.streamlit.app/)
## Pasos a seguir 
Puesto que esta parte es más bien un tanteo para ver si la rpi no sale ardiendo, hay que tener las siguientes cosas en mente: 
-  No voy a reinstalar el SO desde cero,por lo que conserva los paquetes instalados del hailo all aunque no tenga instalado el AI KIT.
- 

Procedo a hacer los siguientes pasos: 
0. En el terminal nativo de la rpi: 
```bash 
#alias temp='/usr/bin/vcgencmd measure_temp' # alias para  leer la temperatura 
#watch -c -d -b -n 30 -- temp
watch -c -b -d  -n 30 -- 'vcgencmd measure_temp'
```
1. En el terminal remoto SSH de la rpi5:
```bash
curl -fsSL https://ollama.com/install.sh | sh # installamos ollama 
ollama run llama3 # instalamos llama 3 por poner un ejemplo 
#se queda atascado por alguna razń 
#pruebo con tiy llama
ollama run tinyllama
##este funciona



```
2. En el terminal de mi portatil(fedora) instalo [open-webui](https://docs.openwebui.com/getting-started/quick-start/) mediante python(conda) aunque debería haber usado docker por reproducibilidad:-> me parece demasiado engorroso  para algo que es simplemanete una pruebecilla 
```bash
conda create -n open-webui python=3.11
conda activate open-webui
pip install open-webui
open-webui serve
```

# Actualización 2025-02-27
 Pareceser que el instalador ollama tiee un buhg que impide que se descargen los modelos correctamente, ver los siguientes enlaces 
 - [8484](https://github.com/ollama/ollama/issues/8484)
 - [comentario utili 1 ](https://github.com/ollama/ollama/issues/8484#issuecomment-2600860889)
- [8406](https://github.com/ollama/ollama/issues/8406)
 
 # Actualización 2026-02-28
 [Exposición de puertos](https://github.com/ollama/ollama/blob/main/docs/faq.md#setting-environment-variables-on-linux)
 Empiezo con una instalación en limpio
 Parece que el problema se corrige en las versiones posteriores 
 voy a provar con la versión v0.5.12
 ```bash 
curl -fsSL https://ollama.com/install.sh | OLLAMA_VERSION=0.5.12 sh
ollama pull llama3.2
#esta vez ha sido un exito 
#procedo a exponer el puerto de ollama para hacer queries
sudo nano /etc/systemd/system/ollama.service
#añado lo siguiente al al final de service
#[Service]
#Environment="OLLAMA_HOST=0.0.0.0:11434"
systemctl daemon-reload
systemctl restart ollama
```
