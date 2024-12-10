import java.util.Scanner;
import java.util.Random;

public class Main { public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);
        Random random = new Random();

        System.out.println("Selecciona el tamaño del mapa (ejemplo: 10 10 para un tablero de 10x10)");

        System.out.println("Columnas:");
        int columnas = scanner.nextInt();

        System.out.println("Filas:");
        int filas = scanner.nextInt();

        int[][] mapa = new int[columnas][filas];

        System.out.println("Indica cuantos bárcos quieres tener");
        int barcos = scanner.nextInt();

        System.out.println("Indica el número de disparos");
        int disparos = scanner.nextInt();

        //generación de barcos
        int direccion_barco_x = 0;
        int direccion_barco_y = 0;

        for (int i = 0; i < barcos; i++) {
            int mitad_tablero = (filas/2);
            int x_o_y = random.nextInt(0,1); // 0 es horizontal 1 es vertical
            direccion_barco_x = random.nextInt(0,1);
            direccion_barco_y = random.nextInt(0,1);
            int posi_x = random.nextInt(0, filas);
            int posi_y = random.nextInt(0, filas);
            mapa[posi_x][posi_y] = 1;

            // 0 izquierda, 1 derecha
            if(posi_x > mitad_tablero){
                direccion_barco_x = 0;
            } else if (posi_y > mitad_tablero) {
                direccion_barco_y = 0;
            } else if (posi_x < mitad_tablero) {
                direccion_barco_x = 1;
            } else if (posi_y < mitad_tablero) {
                direccion_barco_y = 1;
            } else{
                direccion_barco_x = 1;
                direccion_barco_y = 1;
            }
            int size_barco = random.nextInt(1, mitad_tablero);

            for (int j = 0; j < size_barco; j++) {
                int cont_x = 1;
                int cont_y = 1;
                if (direccion_barco_x == 0){
                    cont_x = -1;
                    if (x_o_y == 0){
                        mapa[posi_x][posi_y + cont_y] = 1;
                        cont_y--;
                    } else{
                        mapa[posi_x + cont_x][posi_y] = 1;
                        cont_x--;
                    }
                } else if (direccion_barco_x == 1) {
                    cont_x = 1;
                    if (x_o_y == 0){
                        mapa[posi_x][posi_y + cont_y] = 1;
                        cont_y++;
                    } else{
                        mapa[posi_x + cont_x][posi_y] = 1;
                        cont_x++;
                    }
                } else if (direccion_barco_y == 0){
                    cont_y = -1;
                    if (x_o_y == 0){
                        mapa[posi_x][posi_y + cont_y] = 1;
                        cont_y--;
                    } else{
                        mapa[posi_x + cont_x][posi_y] = 1;
                        cont_x--;
                    }
                } else{
                    cont_y = 1;
                    if (x_o_y == 0){
                        mapa[posi_x][posi_y + cont_y] = 1;
                        cont_y--;
                    } else{
                        mapa[posi_x + cont_x][posi_y] = 1;
                        cont_x--;
                    }
                }
            }
        }
        for (int i = 0; i < mapa.length; i++) {
            for (int j = 0; j < mapa[i].length; j++) {
                System.out.print(mapa[i][j] + "  ");
            }
            System.out.println();
        }

    }
}


v.2
import java.util.Scanner;
import java.util.Random;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Random random = new Random();

        // Configuración inicial
        System.out.println("Selecciona el tamaño del mapa (ejemplo: 10x10)");
        System.out.print("Columnas: ");
        int columnas = scanner.nextInt();
        System.out.print("Filas: ");
        int filas = scanner.nextInt();

        int[][] mapa = new int[filas][columnas];
        boolean[][] disparos = new boolean[filas][columnas];

        System.out.print("Indica cuántos barcos quieres: ");
        int numBarcos = scanner.nextInt();
        System.out.print("Indica el número máximo de disparos: ");
        int maxDisparos = scanner.nextInt();

        // Generar barcos
        for (int i = 0; i < numBarcos; i++) {
            boolean colocado = false;
            while (!colocado) {
                int posX = random.nextInt(filas);
                int posY = random.nextInt(columnas);
                int direccion = random.nextInt(2); // 0 = horizontal, 1 = vertical
                int tamanoBarco = random.nextInt(2, Math.min(filas, columnas) / 2 + 1);

                boolean puedeColocar = true;
                for (int j = 0; j < tamanoBarco; j++) {
                    int x = direccion == 0 ? posX : posX + j;
                    int y = direccion == 0 ? posY + j : posY;
                    if (x >= filas || y >= columnas || mapa[x][y] == 1) {
                        puedeColocar = false;
                        break;
                    }
                }

                if (puedeColocar) {
                    for (int j = 0; j < tamanoBarco; j++) {
                        int x = direccion == 0 ? posX : posX + j;
                        int y = direccion == 0 ? posY + j : posY;
                        mapa[x][y] = 1;
                    }
                    colocado = true;
                }
            }
        }

        // Juego
        int disparosRealizados = 0;
        int barcosRestantes = 0;
        for (int[] fila : mapa) {
            for (int celda : fila) {
                if (celda == 1) barcosRestantes++;
            }
        }

        while (barcosRestantes > 0 && disparosRealizados < maxDisparos) {
            // Imprimir el mapa
            System.out.println("\nTablero:");
            for (int i = 0; i < filas; i++) {
                for (int j = 0; j < columnas; j++) {
                    if (disparos[i][j]) {
                        if (mapa[i][j] == 1) {
                            System.out.print("X  "); // Acierto
                        } else {
                            System.out.print("O  "); // Fallo
                        }
                    } else {
                        System.out.print("~  "); // No descubierto
                    }
                }
                System.out.println();
            }

            // Información del estado
            System.out.println("Disparos restantes: " + (maxDisparos - disparosRealizados));
            System.out.println("Barcos restantes: " + barcosRestantes);

            // Pedir disparo
            System.out.print("Introduce la fila (0 a " + (filas - 1) + "): ");
            int fila = scanner.nextInt();
            System.out.print("Introduce la columna (0 a " + (columnas - 1) + "): ");
            int columna = scanner.nextInt();

            if (fila < 0 || fila >= filas || columna < 0 || columna >= columnas) {
                System.out.println("Coordenadas fuera del tablero. Intenta de nuevo.");
                continue;
            }

            if (disparos[fila][columna]) {
                System.out.println("Ya disparaste aquí. Intenta de nuevo.");
                continue;
            }

            disparos[fila][columna] = true;
            disparosRealizados++;

            if (mapa[fila][columna] == 1) {
                System.out.println("¡Has acertado!");
                mapa[fila][columna] = 0; // Hundir parte del barco
                barcosRestantes--;
            } else {
                System.out.println("¡Has fallado!");
            }
        }

        // Resultado final
        System.out.println("\nTablero final:");
        for (int i = 0; i < filas; i++) {
            for (int j = 0; j < columnas; j++) {
                if (disparos[i][j]) {
                    if (mapa[i][j] == 1) {
                        System.out.print("X  ");
                    } else {
                        System.out.print("O  ");
                    }
                } else {
                    System.out.print("~  ");
                }
            }
            System.out.println();
        }

        if (barcosRestantes == 0) {
            System.out.println("¡Felicidades! Hundiste todos los barcos.");
        } else {
            System.out.println("¡Se acabaron los disparos! Los barcos restantes ganan.");
        }
    }
}

