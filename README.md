# chatbot_offline
Chat bot Off line em python - sistema consegue realizar pedido de uma pizzaria e responde tanto por voz e texto, tudo isso off line


#Desenvolvido por Jonatah Nery de Carvalho#
#necessário fazer download do modelo da linguagem no site <https://alphacephei.com/vosk/models>
#e colar o caminho da pasta em Model 
#para correto funcionamento da função ouvir_microfone em localhost

from vosk import Model, KaldiRecognizer
import pyttsx3
import pyaudio

def ouvir_fala(nome): #Função com a opção de comunicação oral

    while True:
        menu = f'''Olá faça seu pedido {nome}, temos:
        pizza de frango,
        pizza de calabresa e
        pizza de atum,
        lembrando todas as pizzas são tamanho grande 
        caso deseje sair é só falar.
        Qual a sua opção?'''
        gerar_audio(menu)
        fala = ouvir_microfone()

        if "frango" in fala:
                while True:

                    gerar_audio("Seu pedido é uma Pizza de frango?")
                    pedido = ouvir_microfone()
                    
                    if "sim" in pedido or "s" in pedido:
                        gerar_audio("Pedido registrado, aguarde o preparo")
                        break
                    elif "não" in pedido or "n" in pedido or "nao" in pedido:
                        gerar_audio("Pedido não registrado, escolha outro pedido")
                        break
                    else:
                        gerar_audio("Desculpe não entendi, escolha sim ou não")


        elif "calabresa" in fala:
                while True:
                    gerar_audio("Seu pedido é uma Pizza de calabresa?")
                    pedido = ouvir_microfone()
                    
                    if "sim" in pedido or "s" in pedido:
                        gerar_audio("Pedido registrado, aguarde o preparo")
                        break
                    elif "não" in pedido or "n" in pedido or "nao" in pedido:
                        gerar_audio("Pedido não registrado, escolha outro pedido")
                        break
                    else:
                        gerar_audio("Desculpe não entendi, escolha sim ou não")
            
        elif "atum" in fala:
                while True:
                    gerar_audio("Seu pedido é uma Pizza de atum?")
                    pedido = ouvir_microfone()
                    
                    if "sim" in pedido or "s" in pedido:
                        gerar_audio("Pedido registrado, aguarde o preparo")
                        break
                    elif "não" in pedido or "n" in pedido or "nao" in pedido:
                        gerar_audio("Pedido não registrado, escolha outro pedido")
                        break                    
                    else:
                        gerar_audio("Desculpe não entendi, escolha sim ou não")

        elif "sair" in fala:
            gerar_audio("Até logo!")
            break
        elif fala == "a":
            gerar_audio("Desculpe, não entendi o que você disse.")
            continue
        elif fala == "b":
            gerar_audio("Desculpe, estou com problemas para processar a fala.")
            break
        else:
            gerar_audio("Desculpe, não entendi o que você disse.")

        if fala != "b":
            gerar_audio("Deseja continuar fazendo pedidos? (sim ou não): ")
            resposta = ouvir_microfone()
            if resposta == "sim" or resposta == "s":
                continue
            else:
                break
        


def ler_texto(nome): #Função com a opção de escrita

    while True:
        print("\nOlá faça seu pedido, temos:")
        print("Pizza de frango no valor de 40 reais")
        print("Pizza de calabresa no valor de 43 reais")
        print("Pizza de atum no valor de 45 reais")
        print("Ou digite sair")
        print("Obs.: Todas as pizzas são tamanho grande")
        escrita = input(f"{nome}, escolha seu pedido: \n")
    
                
        if "frango" in escrita:
            while True:
                pedido = input("Seu pedido é uma Pizza de frango: \n")

                if "sim" in pedido or "s" in pedido:
                    print("Pedido registrado, aguarde o preparo\n")
                    break
                elif "não" in pedido or "n" in pedido or "nao" in pedido:
                    print("Pedido não registrado, escolha outro pedido\n")
                    break
                else:
                    print("Desculpe não entendi, escolha sim ou não\n")


        elif "calabresa" in escrita:
            while True:
                pedido = input("Seu pedido é uma Pizza de calabresa: \n")
                
                if "sim" in pedido or "s" in pedido:
                    print("Pedido registrado, aguarde o preparo\n")
                    break
                elif "não" in pedido or "n" in pedido or "nao" in pedido:
                    print("Pedido não registrado, escolha outro pedido\n")
                    break
                else:
                    print("Desculpe não entendi, escolha sim ou não\n")
        
        elif "atum" in escrita:
            while True:
                pedido = input("Seu pedido é uma Pizza de atum: \n")
                
                if "sim" in pedido or "s" in pedido:
                    print("Pedido registrado, aguarde o preparo\n")
                    break
                elif "não" in pedido or "n" in pedido or "nao" in pedido:
                    print("Pedido não registrado, escolha outro pedido\n")
                    break                    
                else:
                    print("Desculpe não entendi, escolha sim ou não\n")

        elif "sair" in escrita:
            print("Até logo!\n")
            break
        else:
            print("Desculpe, não entendi o que você disse.\n")

        resposta = input("Deseja continuar fazendo pedidos? (sim/não): \n")
        if resposta == "sim" or resposta == "s":
            continue
        else:
            break
        
    

def ouvir_microfone(): #Função para ouvir microfone padrão e retornar texto
    
    #necessário fazer download do modelo da linguagem no site <https://alphacephei.com/vosk/models> 
    #e colar o caminho da pasta
    model = Model(r"C:\Users\Jonh\Desktop\vosk-model-small-pt-0.3")

    recognizer = KaldiRecognizer(model, 16000)

    cap = pyaudio.PyAudio()
    stream = cap.open(format=pyaudio.paInt16, channels=1, rate=16000, input=True, frames_per_buffer=8192)
    stream.start_stream()
    while True: 
        data = stream.read(4096)
        if recognizer.AcceptWaveform(data):
            text = recognizer.Result()
            print (text[14:-3])            
            return text[14:-3]
                
                
            
            
    
def gerar_audio(text): #recebe texto e transforma em audio além de reproduzi-lo
    engine = pyttsx3.init()
    engine.say(text)
    engine.runAndWait()
    
def main(): #principal
    nome = input('Digite seu nome: ')
    print(f"Olá {nome}!")
    print("Como você prefere se comunicar?")
    while True:
        user_input = input('''
        Digite 1 para ter atendimento por voz
        Digite 2 para ter atendimento por escrita
        Digite 3 para sair: ''')

        try:
            if int(user_input) == 1:
                ouvir_fala(nome)
            elif int(user_input) == 2:
                ler_texto(nome)
            elif int(user_input) == 3:
                break
            else:
                print("Comando inválido.")
                continue

        except ValueError:
            print("Comando inválido.")
            continue

main()
