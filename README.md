# 📌 Projeto RabbitM Atividade 1

Este projeto demonstra um exemplo básico de comunicação entre dois programas em Python usando **RabbitMQ** e a biblioteca **pika**.  
A ferramenta utilizada para desenvolvimento e execução foi o **Visual Studio Code (VS Code)**.

---

## 📤 Código `send.py` (Produtor)

```python
import pika

connection = pika.BlockingConnection(
    pika.ConnectionParameters(host='localhost'))
channel = connection.channel()

channel.queue_declare(queue='hello')

channel.basic_publish(exchange='', routing_key='hello', body='Hello World!')
print(" [x] Sent 'Hello World!'")
connection.close()
````
## 📤 Código `receive.py` (Consumidor)

```python
import pika, sys, os

def main():
    connection = pika.BlockingConnection(pika.ConnectionParameters(host='localhost'))
    channel = connection.channel()

    channel.queue_declare(queue='hello')

    def callback(ch, method, properties, body):
        print(f" [x] Received {body}")

    channel.basic_consume(queue='hello', on_message_callback=callback, auto_ack=True)

    print(' [*] Waiting for messages. To exit press CTRL+C')
    channel.start_consuming()

if __name__ == '__main__':
    try:
        main()
    except KeyboardInterrupt:
        print('Interrupted')
        try:
            sys.exit(0)
        except SystemExit:
            os._exit(0)
````
## Resultado Esperado

Espera-se que a execução dos dois códigos seja de acordo com o que foi demonstrado em classe, com o "receive.py" (consumidor) aguarde a mensagem do "send.py"(produtor), onde a mensagem do send,py que é "Hello World" seja enviada para o cosumidor.

