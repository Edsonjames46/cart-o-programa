import tkinter as tk
from tkinter import messagebox, filedialog
import random
import datetime
import webbrowser  # Para abrir o navegador

# Dicionários ruas e setores (modificados para incluir horários)
ruas = {
    'TP': {'numeros': [10, 50, 70, 210, 350, 450, 520, 570, 610, 435, 475, 515, 575, 305, 375, 5, 45, 65, 85, 105, 165, 195, 215, 245],
          'horarios': ['19:10', '20:15', '00:50', '02:45']},
    'AL': {'numeros': [10, 30, 50, 70, 90, 110],
           'horarios': ['02:15', '11:45', '14:30', '17:15']},
    'AA': {'numeros': [225, 325, 345, 365, 385, 405, 425, 470, 450, 430, 410, 390, 360, 340, 320, 140, 270, 200, 100, 60, 10],
           'horarios': ['08:45', '11:00', '14:15', '17:30']},
    'CA': {'numeros': [40, 25],
           'horarios': ['09:30', '12:15', '15:45']},
    'CE': {'numeros': [205, 285, 315, 355, 385, 410, 390, 349, 320, 300, 280, 260, 240, 220, 200, 180, 150, 110, 10],
           'horarios': ['08:15', '10:45', '13:30', '16:00']},
    'EA': {'numeros': [150, 200, 240, 280, 300, 320, 340, 360, 380, 480, 565, 375, 325, 305, 265, 245, 205, 155, 55, 25, 5],
           'horarios': ['08:30', '11:15', '14:45', '17:00']},
    'EM': {'numeros': [25, 45, 65, 105, 125, 195, 155, 225, 255, 40, 80, 425, 445, 475, 495, 555, 595, 685, 705, 480, 800, 760, 700, 680, 620, 560, 500, 460, 420, 380, 330, 825, 785, 765],
           'horarios': ['09:00', '11:30', '14:00', '16:30']},
    'LA': {'numeros': [80, 50, 10, 105, 45, 5],
           'horarios': ['08:45', '11:00', '14:15', '17:30']},
    'LM': {'numeros': [10, 60, 90, 110, 130, 180, 230, 280, 25],
           'horarios': ['09:15', '11:45', '14:30', '17:15']},
    'LS': {'numeros': [5, 85, 115, 185, 215, 245, 285, 325, 395, 415, 410, 370, 330, 300, 280, 220, 190, 150, 110, 80],
           'horarios': ['08:30', '11:15', '14:45', '17:00']},
    'M': {'numeros': [5, 85, 145, 185, 205, 235, 265, 305, 345, 365, 385, 425, 455, 90, 120, 150, 210, 290, 330, 360, 390, 420, 480, 715, 745, 775, 805, 825, 842, 865, 885, 905, 925, 945, 985, 1005, 620, 660, 680, 2870, 2930, 2690, 2750, 2780, 2810, 2840, 700, 780, 820, 840, 1075, 1105, 1125, 1145, 1165, 1185, 1325, 1365, 1445, 1230, 1260, 1300, 1665, 1805, 1865, 1865, 1935, 1955, 1975, 1995, 2015, 2035, 2075, 2095, 1870, 1910, 1930, 1950, 1970, 2020, 2060, 2090, 2170, 2150],
           'horarios': ['08:00', '10:30', '13:15', '16:45']},
    'MM': {'numeros': [2355, 2415, 2445, 2475, 2340, 2370, 2400, 2480, 2540, 2620],
           'horarios': ['09:30', '12:15', '15:45']},
    'MMM': {'numeros': [2970, 2990, 3030, 3050, 3110, 3160, 3180, 3220, 3260, 2925, 2955, 3040, 3185, 3205, 3225, 3245, 3395, 3335, 3295, 3370, 3440, 3520, 3350, 3570, 3590, 3630, 3670, 3710, 3525, 3575, 3805, 3995, 4015, 4035, 4055, 4075, 4095, 4125, 4145, 4165, 3780],
           'horarios': ['08:45', '11:00', '14:15', '17:30']},
    'MMMM': {'numeros': [3840, 3550, 3910, 3930, 3950, 3970, 4010, 4245, 4325, 4385, 4415, 4445, 4475, 4555, 4575, 4595, 4615, 4635, 4655, 4380, 4400, 4420, 4440, 4480, 4540, 4580, 4650, 4725, 7455, 4785, 4805, 4790, 2075],
           'horarios': ['08:15', '10:45', '13:30', '16:00']},
    'NT': {'numeros': [225, 205, 185, 135, 95, 65, 5, 305, 10, 70, 110, 170, 210, 260, 290, 350, 400, 440, 480, 510, 540, 600, 660, 680, 645, 625, 585, 555, 525, 455],
           'horarios': ['09:00', '11:30', '14:00', '16:30']},
    'NE': {'numeros': [5],
           'horarios': ['08:30', '11:15', '14:45', '17:00']},
    'NO': {'numeros': [30, 50, 90, 110, 150, 135],
           'horarios': ['08:45', '11:00', '14:15', '17:30']},
    'SE': {'numeros': [10, 30, 70, 110, 240, 35, 95, 185],
           'horarios': ['09:00', '11:30', '14:00', '16:30']},
    'SM': {'numeros': [100, 200, 300, 500, 600, 700, 800, 900, 1000],
           'horarios': ['08:00', '10:30', '13:15', '16:45']}
}

# Função para abrir o mapa em uma janela do navegador
def open_map():
    webbrowser.open("https://www.google.com/maps/d/u/0/edit?hl=pt-BR&mid=1OUG1PrU5QnqMfWzy6ejnL7FbmHWS1Bk&ll=-20.004118082195774%2C-43.92939990679362&z=16")

class SetorSelectorApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Seletor de Setores")
        self.setor_selecionado = None
        self.resultados = []
        self.listbox_setores = tk.Listbox(root, selectmode=tk.MULTIPLE, exportselection=False)
        for setor in ["TP", "AL", "AA", "CA", "CE", "EA", "EM", "LA", "LM", "LS", "M", "MM", "MMM", "MMMM", "NT", "NE", "NO", "SE", "SM"]:
            self.listbox_setores.insert(tk.END, setor)
        self.listbox_setores.pack(pady=10)
        self.button_selecionar = tk.Button(root, text="Selecionar Setor(es)", command=self.selecionar_setor)
        self.button_selecionar.pack()
        self.text_resultados = tk.Text(root, height=15, width=60)
        self.text_resultados.pack(pady=10)
        self.button_imprimir = tk.Button(root, text="Imprimir Resultados", command=self.imprimir_resultados)
        self.button_imprimir.pack()
        self.button_salvar = tk.Button(root, text="Salvar Resultados", command=self.salvar_resultados)
        self.button_salvar.pack()
        self.button_sair = tk.Button(root, text="Sair", command=root.destroy)
        self.button_sair.pack()

        # Adicionar o botão para abrir o mapa do MyMaps
        self.button_abrir_mapa = tk.Button(root, text="Abrir Mapa", command=open_map)
        self.button_abrir_mapa.pack(pady=10)

    def selecionar_quatro_numeros_casa_aleatorios(self, rua_data):
        numeros = random.sample(rua_data['numeros'], min(4, len(rua_data['numeros'])))
        horarios = random.sample(rua_data['horarios'], min(4, len(rua_data['horarios'])))
        return numeros, horarios

    def selecionar_setor(self):
        self.setores_selecionados = []
        for i in self.listbox_setores.curselection():
            self.setores_selecionados.append(self.listbox_setor.get(i))
        if not self.setores_selecionados:
            messagebox.showwarning("Aviso", "Por favor, selecione pelo menos um setor.")
            return
        self.resultados = []
        for setor in self.setores_selecionados:
            ruas_do_setor = setores[setor]
            resultado_setor = f"Setor {setor}:\n"
            for rua in ruas_do_setor:
                numeros, horarios = self.selecionar_quatro_numeros_casa_aleatorios(ruas[rua])
                resultado_rua = f"\tRua {rua}: {', '.join(map(str, numeros))}\n"
                resultado_horario = f"\tHorários: {', '.join(horarios)}\n\n"
                resultado_setor += resultado_rua + resultado_horario
            self.resultados.append(resultado_setor)
        messagebox.showinfo("Sucesso", "Resultados gerados com sucesso!")

    def imprimir_resultados(self):
        if not self.resultados:
            messagebox.showwarning("Aviso", "Nenhum resultado foi gerado ainda.")
            return
        self.text_resultados.delete(1.0, tk.END)
        for resultado in self.resultados:
            self.text_resultados.insert(tk.END, resultado + "\n\n")

    def salvar_resultados(self):
        if not self.resultados:
            messagebox.showwarning("Aviso", "Nenhum resultado foi gerado ainda.")
            return
        arquivo = tk.filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Arquivos de Texto", "*.txt")])
        if arquivo:
            with open(arquivo, "w") as f:
                for resultado in self.resultados:
                    f.write(resultado + "\n\n")
            messagebox.showinfo("Sucesso", f"Resultados salvos em {arquivo}")

if __name__ == "__main__":
    root = tk.Tk()
    app = SetorSelectorApp(root)
    root.mainloop()
