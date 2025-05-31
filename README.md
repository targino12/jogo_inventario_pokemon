
# inventario_pokemon.py

class No:
    def __init__(self, dado):
        self.dado = dado
        self.proximo = None

class ListaEncadeada:
    def __init__(self):
        self.inicio = None

    def adicionar(self, item):
        novo_no = No(item)
        if not self.inicio:
            self.inicio = novo_no
        else:
            atual = self.inicio
            while atual.proximo:
                atual = atual.proximo
            atual.proximo = novo_no

    def remover(self, item):
        atual = self.inicio
        anterior = None
        while atual:
            if atual.dado == item:
                if anterior:
                    anterior.proximo = atual.proximo
                else:
                    self.inicio = atual.proximo
                return True
            anterior = atual
            atual = atual.proximo
        return False

    def buscar(self, item):
        atual = self.inicio
        pos = 0
        while atual:
            if atual.dado == item:
                return pos
            atual = atual.proximo
            pos += 1
        return -1

    def listar(self):
        itens = []
        atual = self.inicio
        while atual:
            itens.append(atual.dado)
            atual = atual.proximo
        return itens

def carregar_itens(nome_arquivo, lista):
    with open(nome_arquivo, "r", encoding="utf-8") as arquivo:
        for linha in arquivo:
            lista.adicionar(linha.strip())

def menu():
    print("\n=== INVENTÁRIO DIGITAL DO TAMER ===")
    print("1. Listar Itens")
    print("2. Adicionar Item")
    print("3. Remover Item")
    print("4. Buscar Item")
    print("5. Sair")

def main():
    inventario = ListaEncadeada()
    carregar_itens("itens_pokemon.txt", inventario)

    while True:
        menu()
        escolha = input("Escolha uma opção: ")

        if escolha == "1":
            itens = inventario.listar()
            print("\nItens do inventário:")
            for i, item in enumerate(itens):
                print(f"{i+1}. {item}")

        elif escolha == "2":
            item = input("Digite o nome do novo item: ")
            inventario.adicionar(item)
            print("Item digital adicionado com sucesso!")

        elif escolha == "3":
            item = input("Digite o nome do item a ser removido: ")
            if inventario.remover(item):
                print("Item removido!")
            else:
                print("Item não encontrado.")

        elif escolha == "4":
            item = input("Digite o nome do item que deseja buscar: ")
            pos = inventario.buscar(item)
            if pos >= 0:
                print(f"Item encontrado na posição {pos}.")
            else:
                print("Item não encontrado.")

        elif escolha == "5":
            print("Saindo do inventário. Até a próxima batalha, Tamer!")
            break

        else:
            print("Opção inválida!")

if __name__ == "__main__":
    main()
