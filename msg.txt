import requests, base64, urllib.request, os

TOKEN = "SEU_TOKEN"
REPO = "usuario/repositorio"
FILE = "msg.txt"

def limpar():
    os.system("cls" if os.name == "nt" else "clear")

while True:
    # LER
    url = f"https://raw.githubusercontent.com/{REPO}/main/{FILE}"
    try:
        chat = urllib.request.urlopen(url).read().decode()
    except:
        chat = "erro ao ler"

    print("\n--- CHAT ---")
    print(chat)

    # PERGUNTAR
    msg = input("\nMensagem: ")

    # ENVIAR
    api = f"https://api.github.com/repos/{REPO}/contents/{FILE}"
    r = requests.get(api, headers={"Authorization": f"token {TOKEN}"})
    j = r.json()

    novo = base64.b64decode(j["content"]).decode() + "\n" + msg

    data = {
        "message": "update",
        "content": base64.b64encode(novo.encode()).decode(),
        "sha": j["sha"]
    }

    requests.put(api, json=data, headers={"Authorization": f"token {TOKEN}"})

    # LIMPAR + MOSTRAR ATUALIZADO
    limpar()

    url = f"https://raw.githubusercontent.com/{REPO}/main/{FILE}"
    chat = urllib.request.urlopen(url).read().decode()

    print("\n--- CHAT ATUALIZADO ---")
    print(chat)
