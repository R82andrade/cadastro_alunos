class SistemaCapoeira:
    def __init__(self):
        self.alunos = []
    
    def cadastrar_aluno(self):
        print("\n--- Cadastro de Aluno ---")
        nome = input("Nome do aluno: ").strip()
        idade = input("Idade: ").strip()
        
        while True:
            frequencia = input("Frequência semanal (1 ou 2 vezes): ").strip()
            if frequencia in ['1', '2']:
                break
            print("Por favor, digite 1 ou 2.")
        
        while True:
            origem = input("Origem (1 - Aluno da escola / 2 - Vem de fora): ").strip()
            if origem in ['1', '2']:
                origem = "Da escola" if origem == '1' else "De fora"
                break
            print("Por favor, digite 1 ou 2.")
        
        aluno = {
            'nome': nome,
            'idade': idade,
            'frequencia': int(frequencia),
            'origem': origem,
            'mensalidade': self.calcular_mensalidade(int(frequencia))
        }
        
        self.alunos.append(aluno)
        print(f"Aluno {nome} cadastrado com sucesso!")
    
    def calcular_mensalidade(self, frequencia):
        if frequencia == 1:
            return 100.00
        else:
            return 200.00
    
    def listar_alunos(self):
        print("\n--- Lista de Alunos ---")
        if not self.alunos:
            print("Nenhum aluno cadastrado.")
            return
        
        for i, aluno in enumerate(self.alunos, 1):
            freq = "1 vez/semana" if aluno['frequencia'] == 1 else "2 vezes/semana"
            print(f"{i}. {aluno['nome']} ({aluno['idade']} anos) - {freq} - {aluno['origem']} - R${aluno['mensalidade']:.2f}")
    
    def estatisticas(self):
        if not self.alunos:
            print("\nNenhum aluno cadastrado para gerar estatísticas.")
            return
        
        total = len(self.alunos)
        freq_1 = sum(1 for a in self.alunos if a['frequencia'] == 1)
        freq_2 = total - freq_1
        origem_escola = sum(1 for a in self.alunos if a['origem'] == "Da escola")
        origem_fora = total - origem_escola
        
        print("\n--- Estatísticas Gerais ---")
        print(f"Total de alunos: {total}")
        print(f"\nFrequência:")
        print(f"1x/semana: {freq_1} ({freq_1/total*100:.1f}%)")
        print(f"2x/semana: {freq_2} ({freq_2/total*100:.1f}%)")
        
        print(f"\nOrigem:")
        print(f"Alunos da escola: {origem_escola} ({origem_escola/total*100:.1f}%)")
        print(f"Alunos de fora: {origem_fora} ({origem_fora/total*100:.1f}%)")
        
        # Cálculo da arrecadação total
        total_arrecadado = sum(a['mensalidade'] for a in self.alunos)
        print(f"\nArrecadação total mensal: R${total_arrecadado:.2f}")
        
        # Distribuição 60% professor / 40% escola
        valor_professor = total_arrecadado * 0.6
        valor_escola = total_arrecadado * 0.4
        
        print("\n--- Distribuição Financeira ---")
        print(f"Valor para o professor (60%): R${valor_professor:.2f}")
        print(f"Valor para a escola (40%): R${valor_escola:.2f}")
        
        # Cálculo dos 40% dos alunos (quantidade)
        print("\n--- Distribuição de Alunos (40%) ---")
        qtd_40porcento = int(total * 0.4)
        print(f"40% do total de alunos ({qtd_40porcento} alunos):")
        
        # Ordena por frequência (2x primeiro) e depois por origem (escola primeiro)
        alunos_ordenados = sorted(self.alunos, key=lambda x: (-x['frequencia'], x['origem'] == "Da escola"))
        
        for aluno in alunos_ordenados[:qtd_40porcento]:
            freq = "2x" if aluno['frequencia'] == 2 else "1x"
            print(f"- {aluno['nome']} ({freq}/semana, {aluno['origem']}) - R${aluno['mensalidade']:.2f}")
    
    def menu(self):
        while True:
            print("\n--- Sistema de Capoeira ---")
            print("1. Cadastrar aluno")
            print("2. Listar alunos")
            print("3. Ver estatísticas")
            print("4. Sair")
            
            opcao = input("Escolha uma opção: ").strip()
            
            if opcao == '1':
                self.cadastrar_aluno()
            elif opcao == '2':
                self.listar_alunos()
            elif opcao == '3':
                self.estatisticas()
            elif opcao == '4':
                print("Saindo do sistema...")
                break
            else:
                print("Opção inválida. Tente novamente.")

# Iniciar o sistema
if __name__ == "__main__":
    sistema = SistemaCapoeira()
    sistema.menu()
