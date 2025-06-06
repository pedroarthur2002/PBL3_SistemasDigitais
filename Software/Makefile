C_FILE = main
S_FILE = matrix_io
FILTERS_FILE = filters
TARGET = main

# Diretórios
SRCDIR = src
INCDIR = include

# Flags do compilador
CFLAGS = -c -I$(INCDIR) -lm
ASFLAGS = -I$(INCDIR)

all: $(S_FILE).o $(C_FILE).o $(FILTERS_FILE).o $(TARGET)
	@echo "Compilation complete"

$(S_FILE).o: $(SRCDIR)/$(S_FILE).s
	as $(ASFLAGS) -o $(S_FILE).o $(SRCDIR)/$(S_FILE).s

$(C_FILE).o: $(SRCDIR)/$(C_FILE).c $(INCDIR)/interface.h $(INCDIR)/stb_image.h $(INCDIR)/stb_image_write.h
	gcc $(CFLAGS) -o $(C_FILE).o $(SRCDIR)/$(C_FILE).c

$(FILTERS_FILE).o: $(SRCDIR)/$(FILTERS_FILE).c $(INCDIR)/interface.h
	gcc $(CFLAGS) -o $(FILTERS_FILE).o $(SRCDIR)/$(FILTERS_FILE).c

$(TARGET): $(S_FILE).o $(C_FILE).o $(FILTERS_FILE).o
	gcc -o $(TARGET) $(S_FILE).o $(C_FILE).o $(FILTERS_FILE).o -lm

run: $(TARGET)
	./$(TARGET)

clean:
	rm -f *.o $(TARGET)

clean-images:
	rm -f *.png *.jpg

debug: $(TARGET)
	gdb $(TARGET)
