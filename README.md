# Matrices-
void imprimirMatrizPantalla(String titulo, float[][] matriz) {
  println("--- " + titulo + " ---");
  for (int i = 0; i < matriz.length; i++) {
    for (int j = 0; j < matriz[i].length; j++) {
      print(matriz[i][j] + "\t");
    }
    println();
  }
  println();
}

void imprimirArreglopantalla(String titulo, float[] arreglo) {
  println("--- " + titulo + " ---");
  for (int i = 0; i < arreglo.length; i++) {
    print(arreglo[i] + "  ");
  }
  println("\n");
}

void mostrarEnPantalla() {
  println(" MATRICES INICIALES ");
  imprimirMatrizPantalla("Matriz A (llenado aleatoriamente)", A);
  imprimirMatrizPantalla("Matriz B (llenado por conteo)", B);

  println(" ARREGLOS INICIALES ");
  imprimirArreglopantalla("Arreglo 1 (aleatorio)", arreglo1);
  imprimirArreglopantalla("Arreglo 2 (aleatorio)", arreglo2);

  println(" RESULTADOS DE OPERACIONES");
  imprimirMatrizPantalla("Producto matricial A x B", multiplicacion);
  imprimirMatrizPantalla("Transpuesta de A", traspuesta);

  if (esInvertible) {
    imprimirMatrizPantalla("Inversa de A (metodo Gauss-Jordan)", inversa);
  } else {
    println("Inversa de A: La matriz NO es invertible");
  }

  imprimirArreglopantalla("Arreglo1 x Matriz A", arreglos);
  println("Producto punto Arreglo1 . Arreglo2 = " + int(produpunto));
} 



float [][] MultiplicarMatrices(float[][]matrizA, float [][]matrizB) {
  float [][] resultado =new float [n][n];
  for (int fila=0; fila<n; fila++) {
    for (int columna=0; columna<n; columna++) {
      float suma=0;
      for (int j=0; j<n; j++) {
        suma+= matrizA[fila][j] * matrizB[columna][j];
      }
      resultado[fila][columna]=suma;
    }
  }
  return resultado;
}

float [][]CalcularTranspuesta(float[][]matriz) {
  float [][] transpuesta =new float [n][n];
  for (int fila=0; fila<n; fila++) {
    for (int columna=0; columna<n; columna++) {
      transpuesta[columna][fila] = matriz[fila][columna];
    }
  }
  return transpuesta;
}

float[][] CalcularInversa(float[][] matriz) {
  float[][] aumentar = new float[n][n*2];

  for (int fila = 0; fila < n; fila++) {
    for (int columna = 0; columna < n; columna++) {
      aumentar[fila][columna] = matriz[fila][columna];
    }
    aumentar[fila][n + fila] = 1;
  }

  for (int filaPivote = 0; filaPivote < n; filaPivote++) {

    if (aumentar[filaPivote][filaPivote] < 0.0001) {
      esInvertible = false;
      return null;
    }

    float valorPivote = aumentar[filaPivote][filaPivote];
    for (int columna = 0; columna < n * 2; columna++) {
      aumentar[filaPivote][columna] /= valorPivote;
    }

    for (int fila = 0; fila < n; fila++) {
      if (fila != filaPivote) {
        float factor = aumentar[fila][filaPivote];
        for (int columna = 0; columna < n * 2; columna++) {
          aumentar[fila][columna] -= factor * aumentar[filaPivote][columna];
        }
      }
    }
  }

  esInvertible = true;
  float[][] inversa = new float[n][n];
  for (int fila = 0; fila < n; fila++) {
    for (int columna = 0; columna < n; columna++) {
      inversa[fila][columna] = aumentar[fila][n + columna];
    }
  }
  return inversa;
}

int n=3; 

float [][] A;
float [][] B;
float [] arreglo1;
float [] arreglo2;

float [][] multiplicacion;
float [][] traspuesta;
float [][] inversa;
float produpunto;
float [] arreglos; 
boolean esInvertible;

void setup () {
size(2000,800);
textSize(20);

A = new float [n][n];
B = new float [n][n];
arreglo1 = new float [n];
arreglo2 = new float [n];

MatrizAleatoria(A);
MatrizConteo(B);

ArregloAleatorio(arreglo1);
ArregloAleatorio(arreglo2);

multiplicacion = MultiplicarMatrices(A,B);
traspuesta = CalcularTranspuesta(A);
inversa = CalcularInversa(A);
arreglos = ProduArrxMatrz(arreglo1, A);
produpunto = productoPunto(arreglo1, arreglo2);

mostrarEnPantalla();
}

void draw() {
background(30);
mostrarEnPantalla();
}

void MatrizAleatoria(float[][] matriz) {
  for (int fila = 0; fila < n; fila++) {
    for (int columna = 0; columna < n; columna++) {
      matriz[fila][columna] = random(1, 10);
    }
  }
}

void MatrizConteo(float[][] matriz) {
  int contador = 1;

  for (int fila = 0; fila < n; fila++) {
    for (int columna = 0; columna < n; columna++) {
      matriz[fila][columna] = contador;
      contador = contador + 1;
    }
  }
}

void ArregloAleatorio(float[] arreglo) {
  for (int i = 0; i < n; i++) {
    arreglo[i] = random(1, 10);
  }
}


float[] ProduArrxMatrz(float[] arreglo, float[][] matriz) {
  float[] resultado = new float[n];

  for (int columna = 0; columna < n; columna++) {
    float suma = 0;

    for (int fila = 0; fila < n; fila++) {
      suma = suma + arreglo[fila] * matriz[fila][columna];
    }

    resultado[columna] = suma;
  }

  return resultado;
}

float productoPunto(float[] arreglo1, float[] arreglo2) {
  float suma = 0;

  for (int i = 0; i < n; i++) {
    suma = suma + arreglo1[i] * arreglo2[i];
  }

  return suma;
}


