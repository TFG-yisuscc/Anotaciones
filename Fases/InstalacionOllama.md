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
2. En el terminal de mi portatil(fedora) instalo [open-webui](https://docs.openwebui.com/getting-started/quick-start/) mediante python(conda) aunque debería haber usado docker por reproducibilidad:
```bash
conda create -n open-webui python=3.11
conda activate open-webui
pip install open-webui
open-webui serve
```