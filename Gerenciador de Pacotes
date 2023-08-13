import os
import subprocess
from tkinter import messagebox
from tkinter import ttk
from tkinter import simpledialog
from tkinter.filedialog import askopenfilename
from tkinter.filedialog import *
from PySide6.QtWidgets import *
from PySide6.QtCore import *
from PySide6.QtGui import *
from tkinter import *
import tkinter as tk
from tqdm import tqdm
from typing import List
import sys


class RequirementsManagerApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Gerenciador de Bibliotecas")
        self.root.geometry("800x600")

        self.requirements_path = ""
        self.installed_packages = []

        self.text_edit = tk.Text(self.root)
        self.text_edit.pack(fill="both", expand=True)

        self.buttons_frame = ttk.Frame(self.root)
        self.buttons_frame.pack(fill="x", padx=10, pady=10)

        self.update_list_button = ttk.Button(
            self.buttons_frame, text="Atualizar Lista", command=self.update_list)
        self.update_list_button.pack(side="left", padx=5)

        self.upgrade_all_button = ttk.Button(
            self.buttons_frame, text="Atualizar Tudo", command=self.upgrade_all_packages)
        self.upgrade_all_button.pack(side="left", padx=5)

        self.clear_cache_button = ttk.Button(
            self.buttons_frame, text="Limpar Cache", command=self.clear_cache)
        self.clear_cache_button.pack(side="left", padx=5)

        self.install_package_button = ttk.Button(
            self.buttons_frame, text="Instalar Pacote", command=self.install_package)
        self.install_package_button.pack(side="left", padx=5)

        self.remove_package_button = ttk.Button(
            self.buttons_frame, text="Remover Pacote", command=self.remove_package)
        self.remove_package_button.pack(side="left", padx=5)

        self.search_entry = ttk.Entry(self.root)
        self.search_entry.pack(side="left", padx=5)
        self.search_entry.bind("<Control-f>", self.focus_search_entry)
        self.search_entry.bind("<Escape>", self.cancel_search)

        self.search_button = ttk.Button(
            self.root, text="Buscar", command=self.search_packages)
        self.search_button.pack(side="left", padx=5)

        self.requirements_label = ttk.Label(
            self.root, text="Caminho do requirements.txt:")
        self.requirements_label.pack(
            side="bottom", anchor="w", padx=10, pady=5)

        self.load_list_button = ttk.Button(
            self.root, text="Carregar Lista", command=self.load_list)
        self.load_list_button.pack(side="bottom", anchor="e", padx=10, pady=5)

        self.progress_bar = ttk.Progressbar(
            self.root, orient="horizontal", length=400, mode="determinate")
        self.progress_bar.pack(side="bottom", padx=10, pady=5)

        self.show_log_button = ttk.Button(
            self.buttons_frame, text="Ver Log de Erros", command=self.show_error_log)
        self.show_log_button.pack(side="left", padx=5)

        # Inicialize uma variável para armazenar o log de erros
        self.error_log = ""  
        
        # self.load_list()

    def update_list(self):
        self.installed_packages = self.get_installed_packages()

        # Obter a lista de bibliotecas padrão do Python
        builtin_modules = set(sys.builtin_module_names)

        # Filtrar as bibliotecas instaladas para remover as padrão
        non_standard_packages = [
            pkg for pkg in self.installed_packages if pkg not in builtin_modules]

        self.text_edit.delete("1.0", tk.END)
        self.text_edit.insert(tk.END, "\n".join(non_standard_packages))

    def get_installed_packages(self) -> List[str]:
        try:
            result = os.popen("pip list --format=freeze").read()
            return [line.split("==")[0] for line in result.splitlines()]
        except:
            return []

    def upgrade_all_packages(self):
        try:
            subprocess.run(["pip", "install", "--upgrade", "pip"], check=True)
            outdated_packages = subprocess.run(
                ["pip", "list", "--outdated"], capture_output=True, text=True, check=True)
            outdated_packages = outdated_packages.stdout.strip().split("\n")[
                2:]
            # Define o valor máximo da barra
            self.progress_bar["maximum"] = len(outdated_packages)
            for idx, package in enumerate(outdated_packages):
                package_name = package.split()[0]
                subprocess.run(
                    ["pip", "install", "-U", package_name], check=True)
                # Atualiza o valor da barra de progresso
                self.progress_bar["value"] = idx + 1
                self.root.update()  # Atualiza a GUI
            messagebox.showinfo("Atualização Concluída",
                                "Todas as bibliotecas foram atualizadas.")
        except subprocess.CalledProcessError as e:
            messagebox.showerror(
                "Erro", f"Ocorreu um erro ao atualizar as bibliotecas:\n{e.stderr}")

    def show_error_log(self):
        if self.error_log:
            log_dialog = tk.Toplevel(self.root)
            log_dialog.title("Erro durante a atualização")
            log_text = tk.Text(log_dialog)
            log_text.pack(fill="both", expand=True)
            log_text.insert(tk.END, self.error_log)
        else:
            messagebox.showinfo(
                "Log de Erros", "Nenhum erro foi registrado durante a atualização.")

    def search_packages(self):
        search_term = self.search_entry.get().lower()
        filtered_packages = [
            pkg for pkg in self.installed_packages if search_term in pkg.lower()]
        self.text_edit.delete("1.0", tk.END)
        self.text_edit.insert(tk.END, "\n".join(filtered_packages))

    def load_list(self):
        self.requirements_path = askopenfilename(
            filetypes=[("Text Files", "*.txt")])
        if self.requirements_path:
            with open(self.requirements_path, "r") as file:
                self.text_edit.delete("1.0", tk.END)
                self.text_edit.insert(tk.END, file.read())
                self.requirements_label.config(
                    text=f"Caminho do requirements.txt: {self.requirements_path}")
                self.installed_packages = self.get_installed_packages()

    def install_package(self):
        package_name = simpledialog.askstring(
            "Instalar Pacote", "Digite o nome do pacote:")
        if package_name:
            try:
                command = ["pip", "install", package_name]
                process = subprocess.Popen(
                    command, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)

                self.progress_bar["value"] = 0
                self.progress_bar.update()

                while process.poll() is None:
                    line = process.stdout.readline()
                    if "Downloading" in line:
                        parts = line.split()
                        if len(parts) >= 6:
                            progress = parts[5].strip('%')
                            self.progress_bar["value"] = float(progress)
                            self.progress_bar.update()
                self.progress_bar["value"] = 100

                stdout, stderr = process.communicate()
                if process.returncode == 0:
                    messagebox.showinfo(
                        "Instalação Concluída", f"Pacote '{package_name}' instalado com sucesso!")
                else:
                    messagebox.showerror(
                        "Erro", f"Erro ao instalar o pacote:\n{stderr}")
            except Exception as e:
                messagebox.showerror("Erro", f"Ocorreu um erro:\n{str(e)}")

    def remove_package(self):
        package_name = simpledialog.askstring("Remover Pacote",
                                              "Digite o nome do pacote a ser removido:")
        if package_name:
            try:
                command = ["pip", "uninstall", package_name, "-y"]
                process = subprocess.Popen(
                    command, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)

                self.progress_bar["value"] = 0
                self.progress_bar.update()

                while process.poll() is None:
                    line = process.stdout.readline()
                    if "Removing" in line:
                        parts = line.split()
                        if len(parts) >= 6:
                            progress = parts[5].strip('%')
                            self.progress_bar["value"] = float(progress)
                            self.progress_bar.update()
                self.progress_bar["value"] = 100

                stdout, stderr = process.communicate()
                if process.returncode == 0:
                    messagebox.showinfo(
                        "Remoção Concluída", f"Pacote '{package_name}' removido com sucesso!")
                else:
                    messagebox.showerror(
                        "Erro", f"Erro ao remover o pacote:\n{stderr}")
            except Exception as e:
                messagebox.showerror("Erro", f"Ocorreu um erro:\n{str(e)}")

    def clear_cache(self):
        try:
            subprocess.run(["pip", "cache", "purge"], check=True)
            messagebox.showinfo(
                "Cache Limpo", "O cache do pip foi limpo com sucesso!")
        except subprocess.CalledProcessError as e:
            messagebox.showerror(
                "Erro", f"Erro ao limpar o cache do pip:\n{e.stderr}")

    def focus_search_entry(self, event):
        self.search_entry.focus_set()

    def cancel_search(self, event):
        self.search_entry.delete(0, tk.END)
        self.update_list()

    def run(self):
        self.root.mainloop()


if __name__ == "__main__":
    root = Tk()
    app = RequirementsManagerApp(root)
    app.run()
