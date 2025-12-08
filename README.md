Later there will be a normal description.

1. Клонировать репозиторий
```bash
git clone https://github.com/satnetuser001/wireguard-container.git
````

2. Опционально. Измените номер порта в compose.yml в ports.
3. На клиенте сгенерируйте файлы ключей для сервера и всех клиентов:
```bash
wg genkey | tee server_private.key | wg pubkey > server_public.key
```
```bash
wg genkey | tee client1_private.key | wg pubkey > client1_public.key
```
```bash
wg genkey | tee client2_private.key | wg pubkey > client2_public.key
```
4. Подробно описать значение из какого файла в какую строку wg0.conf файла подставлять