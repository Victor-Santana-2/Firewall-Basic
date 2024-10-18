# Firewall-Basic
# Documentação do Código

## Introdução

Este código é um script de detecção de ataques de rede que utiliza a biblioteca Scapy para capturar e analisar pacotes de rede. O script é capaz de detectar e bloquear IPs que excedem um limite de pacotes por segundo, além de detectar e bloquear o worm Nimda.

## Constantes e Variáveis

- **THRESHOLD**: Limite de pacotes por segundo para considerar um IP como suspeito (40 pacotes por segundo).
- **whitelist_ips**: Conjunto de IPs permitidos (lidos do arquivo `whitelist.txt`).
- **blacklist_ips**: Conjunto de IPs bloqueados (lidos do arquivo `blacklist.txt`).
- **packet_count**: Dicionário que armazena a contagem de pacotes por IP.
- **start_time**: Tempo inicial da captura de pacotes.
- **blocked_ips**: Conjunto de IPs bloqueados.

## Funções

### `read_ip_file(filename)`

- Lê um arquivo de texto que contém IPs, um por linha, e retorna um conjunto de IPs.
- Utilizada para ler os arquivos `whitelist.txt` e `blacklist.txt`.

### `is_nimda_worm(packet)`

- Verifica se um pacote contém a assinatura do worm Nimda (um pedido GET para `/scripts/root.exe`).
- Retorna `True` se o pacote contiver a assinatura, `False` caso contrário.

### `log_event(message)`

- Registra um evento em um arquivo de log.
- Cria o diretório `logs` se não existir e gera um nome de arquivo de log com base na data e hora atual.

### `packet_callback(packet)`

- Função de callback chamada para cada pacote capturado.
- Verifica se o IP do pacote está na whitelist ou blacklist e bloqueia o pacote se necessário.
- Verifica se o pacote contém a assinatura do worm Nimda e bloqueia o pacote se necessário.
- Conta o número de pacotes por IP e bloqueia o IP se o limite for excedido.

## Main

- Verifica se o script está sendo executado com privilégios de root.
- Lê os arquivos `whitelist.txt` e `blacklist.txt` e armazena os IPs em conjuntos.
- Inicia a captura de pacotes com a função `sniff` e passa a função `packet_callback` como callback.

## Exemplo de Uso

- Execute o script como root: `sudo python script.py`
- O script começará a capturar pacotes e bloquear IPs que excedam o limite ou contenham a assinatura do worm Nimda.
- Os eventos serão registrados em arquivos de log no diretório `logs`.
