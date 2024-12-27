import time
import matplotlib.pyplot as plt
from prettytable import PrettyTable

# Fungsi untuk menghitung kontribusi total
def calculate_contribution(player):
    return player["goals"] + player["assists"]

# Pendekatan iteratif
def sort_players_iterative(players):
    sorted_players = players.copy()
    for i in range(len(sorted_players)):
        for j in range(0, len(sorted_players) - i - 1):
            if calculate_contribution(sorted_players[j]) < calculate_contribution(sorted_players[j + 1]):
                sorted_players[j], sorted_players[j + 1] = sorted_players[j + 1], sorted_players[j]
    return sorted_players

# Pendekatan rekursif
def sort_players_recursive(players):
    if len(players) <= 1:
        return players
    pivot = calculate_contribution(players[0])
    greater = [player for player in players[1:] if calculate_contribution(player) >= pivot]
    lesser = [player for player in players[1:] if calculate_contribution(player) < pivot]
    return sort_players_recursive(greater) + [players[0]] + sort_players_recursive(lesser)

# Grafik untuk menyimpan data
n_values = []
iterative_times = []
recursive_times = []

# Fungsi untuk memperbarui grafik
def update_graph():
    plt.figure(figsize=(8, 6))
    plt.plot(n_values, iterative_times, label='Iterative', marker='o', linestyle='-')
    plt.plot(n_values, recursive_times, label='Recursive', marker='o', linestyle='-')
    plt.title('Performance Comparison: Iterative vs Recursive')
    plt.xlabel('Number of Players (n)')
    plt.ylabel('Execution Time (seconds)')
    plt.legend()
    plt.grid(True)
    plt.show()

# Fungsi untuk mencetak tabel waktu eksekusi
def print_execution_table():
    table = PrettyTable()
    table.field_names = ["n", "Iterative Time (s)", "Recursive Time (s)"]
    for i in range(len(n_values)):
        table.add_row([n_values[i], iterative_times[i], recursive_times[i]])
    print(table)

# Program utama
players = []
while True:
    print("\nMenu:")
    print("1. Tambah pemain")
    print("2. Lihat perbandingan waktu pengurutan")
    print("3. Keluar")
    choice = input("Masukkan pilihan Anda (1/2/3): ")

    if choice == "1":
        name = input("Masukkan nama pemain: ")
        goals = int(input("Masukkan jumlah gol: "))
        assists = int(input("Masukkan jumlah assist: "))
        players.append({"name": name, "goals": goals, "assists": assists})
    elif choice == "2":
        if not players:
            print("Tidak ada data pemain. Tambahkan data terlebih dahulu.")
            continue
        
        n_values.append(len(players))

        # Ukur waktu iteratif
        start_time = time.time()
        sort_players_iterative(players)
        iterative_times.append(time.time() - start_time)

        # Ukur waktu rekursif
        start_time = time.time()
        sort_players_recursive(players)
        recursive_times.append(time.time() - start_time)

        # Cetak tabel waktu eksekusi
        print_execution_table()

        # Perbarui grafik
        update_graph()
    elif choice == "3":
        print("Terima kasih telah menggunakan program ini!")
        break
    else:
        print("Pilihan tidak valid. Silakan coba lagi.")
