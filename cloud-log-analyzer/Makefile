# Makefile de la práctica Mini Cloud Log Analyzer
# Comentarios en español para facilitar la comprensión a estudiantes principiantes.

# Variables
AS = aarch64-linux-gnu-as
LD = aarch64-linux-gnu-ld
TARGET = analyzer
SRC = analyzer.s
OBJ = analyzer.o

# Regla principal
all: $(TARGET)

# Compilación
$(TARGET): $(OBJ)
	$(LD) $(OBJ) -o $(TARGET)

$(OBJ): $(SRC)
	$(AS) $(SRC) -o $(OBJ)

# Ejecutar con logs
run: $(TARGET)
	cat logs.txt | ./$(TARGET)

# Limpiar archivos
clean:
	rm -f $(OBJ) $(TARGET)

# Recompilar todo
rebuild: clean all
