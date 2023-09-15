# 1. Lectura del conjunto de datos iris.csv.
from google.colab import files
import pandas as pd
uploaded = files.upload()
file_name = list(uploaded.keys())[0]
iris = pd.read_csv(file_name, sep=",", encoding="utf-8")
iris.head()}
# 2. Clase iterable que instancie un objeto, lea el archivo csv y lo coloque en un dataframe interno, luego muestre los dos siguientes datos cada que se le pidan e imprima error si se han agotado.
data_iterator = DataIterator(iris)
class DataIterator:
    def __init__(self, data):
        self.data = data
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index < len(self.data):
            row = self.data.iloc[self.index]
            self.index += 1
            return row
        else:
            raise StopIteration("error")
# Prueba de la clase
for _ in range(1):
    try:
        row = next(data_iterator)
        print(row)
    except StopIteration as e:
        print(e)     
# 3. Clase iterable que instancie un objeto, lea un conjunto de datos numéricos, instancie los datos en un dataframe de Pandas, implemente un método que calcule las estadísticas descriptivas básicas de cada una de las variables, implemente un método que entregue los nombre de las variables disponibles, implemente un método que reciba el nombre de una variable disponible dentro del objeto y regrese: sus estadísticas básica.
import matplotlib.pyplot as plt
class DescriptiveStatistics:
    def __init__(self, data):
        self.data = data

    def calculate_statistics(self, variable_name):
        if variable_name in self.data.columns:
            variable_data = self.data[variable_name]
            mean = variable_data.mean()
            median = variable_data.median()
            std = variable_data.std()
            q25 = variable_data.quantile(0.25)
            q50 = variable_data.quantile(0.50)
            q75 = variable_data.quantile(0.75)

            # Dibujar un histograma
            plt.hist(variable_data, bins=20, edgecolor='k')
            plt.title(f'Histograma de {variable_name}')
            plt.xlabel(variable_name)
            plt.ylabel('Frecuencia')
            plt.show()

            return {
                "Media": mean,
                "Mediana": median,
                "Desviación Estándar": std,
                "Percentil 25": q25,
                "Percentil 50": q50,
                "Percentil 75": q75
            }
        else:
            return "Variable no encontrada en el conjunto de datos."
stats = DescriptiveStatistics(data)
# Prueba de la clase
variable_name = "sepal_length"
result = stats.calculate_statistics(variable_name)
print(f"Estadísticas para {variable_name}:\n{result}")

