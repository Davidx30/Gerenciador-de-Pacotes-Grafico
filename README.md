Gerenciador de Bibliotecas com Tkinter
Descrição
Este é um programa de gerenciamento de bibliotecas Python que permite listar, atualizar, instalar e remover bibliotecas usando a interface gráfica Tkinter. Ele utiliza comandos do pip para executar essas ações diretamente no terminal. O programa exibe a lista de bibliotecas instaladas, permite a busca por bibliotecas específicas, carrega um arquivo requirements.txt para exibir e gerenciar bibliotecas em um projeto, e fornece a capacidade de instalar e remover bibliotecas individualmente. A barra de progresso mostra o progresso das operações de instalação e remoção.

Como Usar
Execute o script Python.
A interface gráfica Tkinter será aberta.
Use os botões para realizar as seguintes ações:
Atualizar Lista: Lista as bibliotecas instaladas.
Atualizar Tudo: Atualiza todas as bibliotecas instaladas para suas versões mais recentes.
Limpar Cache: Limpa o cache do pip.
Instalar Pacote: Permite instalar uma nova biblioteca digitando seu nome.
Remover Pacote: Permite remover uma biblioteca instalada digitando seu nome.
Buscar: Filtra as bibliotecas instaladas com base em um termo de busca.
Carregar Lista: Carrega um arquivo requirements.txt e lista suas bibliotecas.
A barra de progresso mostra o progresso das operações de atualização e instalação/remoção.
Requisitos
Python 3.x
Bibliotecas: subprocess, tkinter, tkinter.messagebox, tkinter.ttk, tkinter.simpledialog, tkinter.filedialog, tkinter as tk, tqdm
Notas
Certifique-se de executar o programa em um ambiente Python onde o pip esteja instalado e configurado corretamente.
Sempre verifique os termos de uso das bibliotecas que você está instalando ou removendo.
Este programa foi criado com fins educativos e não deve ser usado para atividades maliciosas.
