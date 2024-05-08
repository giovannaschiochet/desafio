def depositar(saldo, valor, extrato):
    if valor > 0:
        saldo += valor
        extrato += f"Depósito: R$ {valor:.2f}"
        print(" Depósito realizado!")
    else:
        print("Operação falhou")

    return saldo, extrato


def sacar(*, saldo, valor, extrato, limite, numero_saques, limite_saques):
    

    if valor > saldo:
        print(" Saldo suficiente.")
    elif saldo > limite:
        print("O valor excedeu o limite.")
    elif numero_saques >= limite_saques:
        print("Número de saques excedido.")
    elif valor > 0:
        saldo -= valor
        extrato += f"Saque: R$ {valor:.2f}\n"
        numero_saques += 1
        print("Saque realizado!")
    else:
        print("O valor inválido.")

    return saldo, extrato


def exibir_extrato(saldo, /, *, extrato):
    print("_____________EXTRATO_________________")
    print("Não há movimentações." if not extrato else extrato)
    print(f"Saldo: R$ {saldo:.2f}")
    print("____________________________-")


def criar_usuario(usuarios):
    cpf = input("Informe o CPF: ")
    usuario = filtrar_usuario(cpf, usuarios)

    if usuario:
        print("CPF já cadastrado!")
        return

    nome = input("Nome completo: ")
    data_nascimento = input("Data de nascimento: ")
    endereco = input("Endereço (logradouro, numero - bairro - cidade/sigla estado): ")

    usuarios.append({"nome": nome, "data_nascimento": data_nascimento, "cpf": cpf, "endereco": endereco})

    print("Usuário criado!")


def filtrar_usuario(cpf, usuarios):
    usuarios_filtrados = [usuario for usuario in usuarios if usuario["cpf"] == cpf]
    return usuarios_filtrados[0] if usuarios_filtrados else None


def criar_conta(agencia, numero_conta, usuarios):
    cpf = input("Informe o CPF: ")
    usuario = filtrar_usuario(cpf, usuarios)

    if usuario:
        print("Conta criada")
        return {"agencia": agencia, "numero_conta": numero_conta, "usuario": usuario}

    print("Usuário desconhecido")


def principal():
    LIMITE_SAQUES = 3
    AGENCIA = "0001"

    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0
    usuarios = []
    contas = []

    while True:
        opcao = menu()

        if opcao == "1":
            valor = float(input("Informe o valor: "))

            saldo, extrato = depositar(saldo, valor, extrato)

        elif opcao == "2":
            valor = float(input("Informe o valor: "))

            saldo, extrato = sacar(
                saldo=saldo,
                valor=valor,
                extrato=extrato,
                limite=limite,
                numero_saques=numero_saques,
                limite_saques=LIMITE_SAQUES,
            )

        elif opcao == "3":
            exibir_extrato(saldo, extrato=extrato)
        
        elif opcao == "4":
            numero_conta = len(contas) + 1
            conta = criar_conta(AGENCIA, numero_conta, usuarios)

            if conta:
                contas.append(conta)
        
        elif opcao == "5":
            criar_usuario(usuarios)

        
        elif opcao == "0":
            break

        else:
            print("Inválido.")


principal()
