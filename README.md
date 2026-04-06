# ransomware
Projeto DIO - Simulação de um ataque ransomware (criptografando e descriptografando os arquivos)

Analista responsável: Rubens Data: 11 de Março de 2026 Tecnologias: Python 3.13, Biblioteca Cryptography (Fernet/AES-128), VS Code.

1. Objetivo do Projeto

Demonstrar o ciclo de vida de um ataque de ransomware, focando na criptografia simétrica de arquivos locais e na posterior implementação de um plano de contingência para restauração dos dados através de um script de descriptografia.

2. Descrição do Ambiente de Teste

• Diretório Alvo: C:\lab\malware\test_files
• Sistema de Arquivos: Windows Local (Excluído do cache de sincronização de nuvem para integridade dos testes).
• Mecanismo de Criptografia: Algoritmo Fernet, que utiliza AES de 128 bits no modo CBC com autenticação HMAC usando SHA256.

3. Execução do "Ataque" (ransomware.py)

O script foi desenvolvido para percorrer o diretório especificado, gerar uma chave única (chave.key) e sobrescrever o conteúdo de todos os arquivos .txt com dados cifrados.
• Desafio Identificado: Durante os testes, observou-se que arquivos não salvos no buffer do editor (VS Code) resultavam em arquivos de 0 bytes após a criptografia.
• Correção: Implementação de protocolo de salvamento físico em disco antes da execução do binário Python.

4. Protocolo de Recuperação (descriptografar.py)

Para a restauração, foi desenvolvido um script que utiliza a mesma chave simétrica gerada no ataque.
• Tratamento de Erros: O script foi blindado com blocos try-except para evitar corrupção de arquivos caso a chave estivesse incorreta (erro de InvalidToken).
• Validação de Integridade: Implementação de funções para leitura em memória antes da escrita em disco, garantindo que o arquivo original só fosse alterado se a descriptografia fosse validada com sucesso.

5. Lições Aprendidas e Conclusões de Auditoria

Custódia de Chaves: A perda do arquivo chave.key tornaria os dados permanentemente irrecuperáveis, destacando a importância de backups de chaves em ambientes corporativos.
Interferência de Software: Identificou-se que a sincronização de nuvem (OneDrive) e o cache de editores podem mascarar o estado real dos dados em disco, sendo necessária a validação via terminal ou explorador de arquivos.
Higiene de Dados: A prática confirmou que o tratamento adequado de exceções no código evita o "apagamento" acidental de dados (arquivos de 0 bytes) durante processos de transformação
