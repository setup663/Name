

import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QWidget, QLabel, QLineEdit, QPushButton, QVBoxLayout
from PyQt5.QtChart import QChart, QChartView, QPieSeries

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Регистрация")
        
        self.central_widget = QWidget(self)
        self.setCentralWidget(self.central_widget)
        
        self.layout = QVBoxLayout()
        self.central_widget.setLayout(self.layout)
        
        self.chart_view = QChartView(self)
        self.layout.addWidget(self.chart_view)
        
        self.username_label = QLabel("Имя пользователя")
        self.layout.addWidget(self.username_label)
        
        self.username_input = QLineEdit()
        self.layout.addWidget(self.username_input)
        
        self.submit_button = QPushButton("Зарегистрироваться")
        self.submit_button.clicked.connect(self.register_user)
        self.layout.addWidget(self.submit_button)
        
        self.chart = QChart()
        self.chart_view.setChart(self.chart)
        
    def register_user(self):
        username = self.username_input.text()
        
        # Сохранение данных в базе данных
        
        # Построение круговой диаграммы по данным о налогах
        self.update_chart()
        
    def update_chart(self):
        # Получение данных о налогах из файла
        tax_data = self.read_tax_data()
        
        # Очистка диаграммы
        self.chart.removeAllSeries()
        
        # Создание серии для круговой диаграммы
        series = QPieSeries()
        
        # Добавление данных о налогах в серии
        for tax, amount in tax_data.items():
            series.append(tax, amount)
        
        # Добавление серии в диаграмму
        self.chart.addSeries(series)
        
        # Показ круговой диаграммы
        self.chart_view.show()
    
    def read_tax_data(self):
        tax_data = {}
        with open("taxes.txt", "r") as file:
            for line in file:
                tax, amount = line.strip().split(":")
                tax_data[tax] = int(amount)
        return tax_data

if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
   sys.exit(app.exec())
