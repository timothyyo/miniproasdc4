# miniproasdc4
Nama:Joshua Timothy NIM:2309116070

import math

class Node:
    def __init__(self, kendaraan):
        self.kendaraan = kendaraan
        self.next = None

class Kendaraan:
    def __init__(self, id, merk, model, tahun, harga):
        self.id = id
        self.merk = merk
        self.model = model
        self.tahun = tahun
        self.harga = harga

class LinkedList:
    def __init__(self):
        self.head = None

    def tambah_di_awal(self, kendaraan):
        new_node = Node(kendaraan)
        new_node.next = self.head
        self.head = new_node

    def tambah_di_akhir(self, kendaraan):
        new_node = Node(kendaraan)
        if not self.head:
            self.head = new_node
            return
        last_node = self.head
        while last_node.next:
            last_node = last_node.next
        last_node.next = new_node

    def hapus_di_awal(self):
        if not self.head:
            print("Katalog kendaraan kosong.")
            return
        self.head = self.head.next

    def hapus_di_akhir(self):
        if not self.head:
            print("Katalog kendaraan kosong.")
            return
        if not self.head.next:
            self.head = None
            return
        second_last = self.head
        while second_last.next.next:
            second_last = second_last.next
        second_last.next = None

    def quick_sort(self, attribute='id'):
        kendaraan_list = self._linked_list_to_list()

        if attribute == 'id':
            key_function = lambda kendaraan: kendaraan.id
        elif attribute == 'merk':
            key_function = lambda kendaraan: kendaraan.merk
        else:
            print("Sorting attribute not supported.")
            return

        def partition(start, end):
            pivot = kendaraan_list[end]
            i = start - 1
            for j in range(start, end):
                if key_function(kendaraan_list[j]) <= key_function(pivot):
                    i += 1
                    kendaraan_list[i], kendaraan_list[j] = kendaraan_list[j], kendaraan_list[i]
            kendaraan_list[i + 1], kendaraan_list[end] = kendaraan_list[end], kendaraan_list[i + 1]
            return i + 1

        def quick_sort_recursive(start, end):
            if start < end:
                partition_index = partition(start, end)
                quick_sort_recursive(start, partition_index - 1)
                quick_sort_recursive(partition_index + 1, end)

        quick_sort_recursive(0, len(kendaraan_list) - 1)

        self._list_to_linked_list(kendaraan_list)

    def _linked_list_to_list(self):
        kendaraan_list = []
        current_node = self.head
        while current_node:
            kendaraan_list.append(current_node.kendaraan)
            current_node = current_node.next
        return kendaraan_list

    def _list_to_linked_list(self, kendaraan_list):
        self.head = None
        for kendaraan in kendaraan_list:
            self.tambah_di_akhir(kendaraan)

    def tampilkan_katalog(self):
        current_node = self.head
        if not current_node:
            print("Katalog kendaraan kosong.")
            return
        print("=== KATALOG KENDARAAN ===")
        while current_node:
            print("ID:", current_node.kendaraan.id)
            print("Merk:", current_node.kendaraan.merk)
            print("Model:", current_node.kendaraan.model)
            print("Tahun:", current_node.kendaraan.tahun)
            print("Harga:", current_node.kendaraan.harga)
            print("------------------------")
            current_node = current_node.next

    def cari_kendaraan(self, id):
        current_node = self.head
        while current_node:
            if current_node.kendaraan.id == id:
                return current_node.kendaraan
            current_node = current_node.next
        return None

    def hapus_by_id(self, id):
        if not self.head:
            print("Katalog kendaraan kosong.")
            return
        if self.head.kendaraan.id == id:
            self.head = self.head.next
            return
        current_node = self.head
        while current_node.next:
            if current_node.next.kendaraan.id == id:
                current_node.next = current_node.next.next
                return
            current_node = current_node.next
        print("Kendaraan dengan ID {} tidak ditemukan.".format(id))

    def update_by_id(self, id, kendaraan_baru):
        current_node = self.head
        while current_node:
            if current_node.kendaraan.id == id:
                current_node.kendaraan = kendaraan_baru
                return
            current_node = current_node.next
        print("Kendaraan dengan ID {} tidak ditemukan.".format(id))

    def create_kendaraan(self, kendaraan):
        self.tambah_di_akhir(kendaraan)
        print("Kendaraan baru telah ditambahkan.")

    @staticmethod
    def generate_default_vehicle_data():
        kendaraan1 = Kendaraan("001", "Toyota", "Avanza", 2020, 150000000)
        kendaraan2 = Kendaraan("002", "Honda", "CR-V", 2019, 200000000)
        kendaraan3 = Kendaraan("003", "Suzuki", "Ertiga", 2021, 180000000)
        kendaraan4 = Kendaraan("004", "Mitsubishi", "Xpander", 2018, 170000000)
        kendaraan5 = Kendaraan("005", "Nissan", "Grand Livina", 2017, 160000000)
    
        default_data = [kendaraan1, kendaraan2, kendaraan3, kendaraan4, kendaraan5]
    
        return default_data

    def jump_search(self, attributes, values):
        if not self.head:
            print("Katalog kendaraan kosong.")
            return None

        key_functions = {}
        for i, attr in enumerate(attributes):
            if attr == 'id':
                key_functions[attr] = lambda kendaraan: kendaraan.id
            elif attr == 'merk':
                key_functions[attr] = lambda kendaraan: kendaraan.merk
            

        n = 0
        current_node = self.head
        while current_node:
            n += 1
            current_node = current_node.next

        jump = int(math.sqrt(n))
        prev_node = None
        current_node = self.head
        while current_node:
            all_values_match = True
            for attr, value, key_function in zip(attributes, values, key_functions.values()):
                if key_function(current_node.kendaraan) != value:
                    all_values_match = False
                    break
            if all_values_match:
                return current_node.kendaraan

            prev_node = current_node
            for _ in range(min(jump, n)):
                if current_node.next:
                    current_node = current_node.next
            n -= jump

        while prev_node:
            all_values_match = True
            for attr, value, key_function in zip(attributes, values, key_functions.values()):
                if key_function(prev_node.kendaraan) != value:
                    all_values_match = False
                    break
            if all_values_match:
                return prev_node.kendaraan
            prev_node = prev_node.next

        print("Kendaraan dengan atribut yang dicari tidak ditemukan.")
        return None

if __name__ == "__main__":
    dealer = LinkedList()
    default_data = LinkedList.generate_default_vehicle_data()
    for kendaraan in default_data:
        dealer.tambah_di_akhir(kendaraan)

    while True:
        print("\n=== MENU ===")
        print("1. Tampilkan Katalog")
        print("2. Tambah Kendaraan di Awal")
        print("3. Tambah Kendaraan di Akhir")
        print("4. Cari Kendaraan")
        print("5. Hapus Kendaraan di Awal")
        print("6. Hapus Kendaraan di Akhir")
        print("7. Hapus Kendaraan by ID")
        print("8. Update Kendaraan by ID")
        print("9. Keluar")

        choice = input("Pilih operasi yang ingin Anda lakukan: ")

        if choice == "1":
            dealer.tampilkan_katalog()
        elif choice == "2":
            id = input("Masukkan ID kendaraan: ")
            merk = input("Masukkan merk kendaraan: ")
            model = input("Masukkan model kendaraan: ")
            tahun = int(input("Masukkan tahun kendaraan: "))
            harga = int(input("Masukkan harga kendaraan: "))
            kendaraan = Kendaraan(id, merk, model, tahun, harga)
            dealer.tambah_di_awal(kendaraan)
        elif choice == "3":
            id = input("Masukkan ID kendaraan: ")
            merk = input("Masukkan merk kendaraan: ")
            model = input("Masukkan model kendaraan: ")
            tahun = int(input("Masukkan tahun kendaraan: "))
            harga = int(input("Masukkan harga kendaraan: "))
            kendaraan = Kendaraan(id, merk, model, tahun, harga)
            dealer.tambah_di_akhir(kendaraan)
        elif choice == "4":
            print("Pilih atribut-atribut yang ingin Anda cari:")
            print("1. ID")
            print("2. Merk")
            search_choice = input("Pilih atribut pencarian: ")
            attributes = []
            if search_choice == "1":
                attributes.append('id')
            elif search_choice == "2":
                attributes.append('merk')
            else:
                print("Pilihan tidak valid.")
                continue
            value = input("Masukkan nilai atribut: ")
            found_kendaraan = dealer.jump_search(attributes, [value])
            if found_kendaraan:
                print("Kendaraan ditemukan:")
                print("ID:", found_kendaraan.id)
                print("Merk:", found_kendaraan.merk)
                print("Model:", found_kendaraan.model)
                print("Tahun:", found_kendaraan.tahun)
                print("Harga:", found_kendaraan.harga)
            else:
                print("Kendaraan tidak ditemukan.")
        elif choice == "5":
            dealer.hapus_di_awal()
            print("Kendaraan di awal telah dihapus.")
        elif choice == "6":
            dealer.hapus_di_akhir()
            print("Kendaraan di akhir telah dihapus.")
        elif choice == "7":
            id = input("Masukkan ID kendaraan yang ingin dihapus: ")
            dealer.hapus_by_id(id)
        elif choice == "8":
            id = input("Masukkan ID kendaraan yang ingin diperbarui: ")
            merk = input("Masukkan merk kendaraan baru: ")
            model = input("Masukkan model kendaraan baru: ")
            tahun = int(input("Masukkan tahun kendaraan baru: "))
            harga = int(input("Masukkan harga kendaraan baru: "))
            kendaraan_baru = Kendaraan(id, merk, model, tahun, harga)
            dealer.update_by_id(id, kendaraan_baru)
        elif choice == "9":
            print("Terima kasih! Program selesai.")
            break
        else:
            print("Pilihan tidak valid. Silakan pilih 1-9.")
