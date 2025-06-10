# corporate-information-system-Project

This project is a robust and intuitive desktop application built with **PyQt5** designed for basic employee data management within an organizational context. It serves as a compact "Corporate Information System" focusing on personnel records. The system allows users to efficiently manage employee information by providing functionalities to search for existing personnel records and add new employees to the database. It aims to streamline basic HR or administrative tasks by offering a straightforward and user-friendly graphical interface. 

# Project Codes

import sys
import sqlite3
from PyQt5.QtWidgets import QWidget, QApplication, QTextEdit, QLabel, QPushButton, QVBoxLayout, QHBoxLayout, QLineEdit


class Pencere (QWidget) :

    def __init__(self):

        super(). __init__()

        self.init_ui()

        self.baglanti_olustur()


    def baglanti_olustur (self) :

        self.baglanti = sqlite3.connect("kurum.db")

        self.cursor = self.baglanti.cursor ()

        sorgu = "Create Table If not exists personel (İsim TEXT,Soyisim TEXT, Unvan TEXT, Sicil_No TEXT)"

        self.cursor.execute(sorgu)

        self.baglanti.commit()


    def init_ui(self) :

        self.sicil_no = QLineEdit ("Sicil Numarasını Giriniz : ")
        self.sorgula = QPushButton ("Sorgula")
        self.temizle = QPushButton ("Temizle")
        self.yazi_alani = QLabel ("")


        self.isim = QLineEdit("İsim Giriniz : ")
        self.soyisim = QLineEdit ("Soyisim Giriniz : ")
        self.unvan= QLineEdit ("Ünvan Giriniz : ")
        self.sicil_no_ekle = QLineEdit ("Sicil No Giriniz : ")
        self.personel_ekle = QPushButton ("Personel Ekle")


        v_box = QVBoxLayout ()

        v_box.addWidget(self.sicil_no)
        v_box.addWidget(self.yazi_alani)
        v_box.addWidget(self.sorgula)

        v_box.addWidget(self.personel_ekle)

        v_box.addWidget(self.isim)
        v_box.addWidget(self.soyisim)
        v_box.addWidget(self.unvan)
        v_box.addWidget(self.sicil_no_ekle)


        v_box.addWidget(self.temizle)

        h_box = QHBoxLayout ()

        h_box.addStretch()
        h_box.addLayout(v_box)
        h_box.addStretch()


        self.setLayout (h_box)

        self.setWindowTitle("Personel Sorgulama")

        self.sorgula.clicked.connect (self.login)

        self.temizle.clicked.connect (self.ekrani_temizle)

        self.personel_ekle.clicked.connect (self.yeni_personel)

        self.show ()


    def login(self):

        sicil = self.sicil_no.text()

        sorgu = "Select * From personel where Sicil_No = ?"

        self.cursor.execute(sorgu,(sicil,))

        data = self.cursor.fetchall ()

        if len(data) == 0 :

            self.yazi_alani.setText("Böyle bir personel bulunamadı\nLütfen yeni bir sicil numarası giriniz")

        else :

            for i in data :

                isim,soyisim,unvan,sicil_no = i

                bilgi = f"İsim  :  {isim}\nSoyisim  :  {soyisim}\nÜnvan  :  {unvan}\nSicil No  :  {sicil_no}"

                self.yazi_alani.setText(bilgi)


    def ekrani_temizle(self):

        self.yazi_alani.clear()


    def yeni_personel(self):

        isim = self.isim.text()
        soyisim = self.soyisim.text()
        ünvan = self.unvan.text()
        sicil = self.sicil_no_ekle.text()

        if isim and soyisim and ünvan and sicil :

            sorgu = "Insert into personel Values (?,?,?,?)"

            self.cursor.execute(sorgu,(isim,soyisim,ünvan,sicil))

            self.baglanti.commit()

            self.yazi_alani.setText("Yeni personel başarıyla eklendi ")

        else :

            self.yazi_alani.setText("Lütfen tüm alanları doldurunuz")


app =QApplication (sys.argv)

pencere = Pencere ()

sys.exit(app.exec_())

# Contact Me

hasankilickaya44@gmail.com

https://github.com/kilickayahasan/kilickayahasan
