# cod
ventos = []
usuarios = []

def cadastro_usuario():
    print(' CADASTRO DE USUÁRIO ')
    nome = input('Digite seu nome completo: ')
    email = input('Digite seu endereço de e-mail: ')
    senha = input('Crie uma senha: ')
    confirmar_senha = input('Confirme sua senha: ')

    if senha != confirmar_senha:
        print(' As senhas não coincidem. Tente novamente.')
        return

    for usuario in usuarios:
        if usuario['email'] == email:
            print('E-mail já em uso. Escolha outro.')
            return

    usuarios.append({"nome": nome, "senha": senha, "email": email})
    print(' Usuário {nome} cadastrado com sucesso!')

def login():
    print('=== LOGIN ===')
    email = input('Insira seu e-mail: ')
    senha = input('Insira sua senha: ')

    for usuario in usuarios:
        if usuario['email'] == email and usuario['senha'] == senha:
            print(' Bem-vindo(a), {usuario["nome"]}!')
            return usuario
    print(' E-mail ou senha inválidos. Tente novamente.')
    return None

def criar_evento(usuario):
    print('=== CRIAR EVENTO ===')
    nome = input('Digite o nome do evento: ')
    descricao = input('Descreva o evento (brevemente): ')
    local = input('Onde o evento será realizado? ')
    data = input('Informe a data do evento (formato: dd/mm/aaaa): ')

    try:
        valor = float(input('Quanto custa a inscrição? (R$): '))
    except ValueError:
        print('Valor inválido. O evento não foi criado.')
        return

    evento_id = len(eventos) + 1
    eventos.append({
        "id": evento_id,
        "nome": nome,
        "descricao": descricao,
        "local": local,
        "data": data,
        "valor": valor,
        "criador": usuario["email"],
        "participantes": []
    })
    print(f' Evento "{nome}" criado com sucesso!')

def listar_eventos():
    print('=== EVENTOS DISPONÍVEIS ===')
    if len(eventos) == 0:
        print('Nenhum evento foi cadastrado ainda.')
        return

    for evento in eventos:
        print(f"ID: {evento['id']}, Nome: {evento['nome']}, Local: {evento['local']}, Data: {evento['data']}")

def remover_evento(usuario):
    listar_eventos()
    if eventos == []:
        print('Não há eventos para remover.')
        return

    try:
        evento_id = int(input('Digite o ID do evento que deseja remover: '))
    except ValueError:
        print('ID inválido. Tente novamente.')
        return

    for evento in eventos:
        if evento['id'] == evento_id and evento['criador'] == usuario["email"]:
            eventos.remove(evento)
            print(f'Evento "{evento["nome"]}" removido com sucesso!')
            return
    print(' Evento não encontrado ou você não tem permissão para removê-lo.')

def menu():
    usuario_logado = None

    while True:
        print('MENU PRINCIPAL')
        print('1. Cadastrar Usuário')
        print('2. Fazer Login')
        print('3. Criar Evento')
        print('4. Listar Eventos')
        print('5. Remover Evento')
        print('6. Sair')

        opcao = input('Escolha uma opção: ')
        if opcao == '1':
            cadastro_usuario()
        elif opcao == '2':
            usuario_logado = login()
        elif opcao == '3':
            if usuario_logado:
                criar_evento(usuario_logado)
            else:
                print(' Por favor, faça login para criar um evento.')
        elif opcao == '4':
            listar_eventos()
        elif opcao == '5':
            if usuario_logado:
                remover_evento(usuario_logado)
            else:
                print(' Por favor, faça login para remover um evento.')
        elif opcao == '6':
            print('Saindo... Até a próxima!')
            break
        else:
            print(' Opção inválida. Tente novamente.')

menu()
