# Cinemas

import java.util.Scanner;

public class Cinema {

    public static void main(String[] args) {
        // Write your code here
        Scanner scanner = new Scanner(System.in);
        boolean isStopped = false;
        boolean ticketSelected = false;
        int row1 = 0;
        int col1 = 0;
        int ticketPrice;
        System.out.println("Enter the number of rows:");
        int row = scanner.nextInt();
        System.out.println("Enter the number of seats in each row:");
        int col = scanner.nextInt();
        int totalOfSeats = row * col;
        int seatPrice = 10;
        int nPurchasedTicket = 0;
        double percentage;
        int currentIncome = 0;
        int totalIncome;
        System.out.println();
        char[][] cinemas = new char[row][col];
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < col; j++) {
                cinemas[i][j] = 'S';
            }
        }

        // check ticket price
        int halfRow = 0;
        if (totalOfSeats > 60) {
            halfRow = (row % 2 == 0) ? (row / 2) : (row - 1) / 2;
        }

        while (!isStopped) {
            System.out.println("""
                    1. Show the seats
                    2. Buy a ticket
                    3. Statistics
                    0. Exit""");
            int n = scanner.nextInt();
            System.out.println();
            switch (n) {
                case 1 -> { // show the seats
                    System.out.println("Cinema:");
                    System.out.print("  ");
                    for (int i = 0; i < col; i++) {
                        System.out.print(i + 1 + " ");
                    }
                    System.out.println();
                    for (int i = 0; i < row; i++) {
                        System.out.print(i + 1 + " ");
                        for (int j = 0; j < col; j++) {
                            System.out.print(cinemas[i][j] + " ");
                        }
                        System.out.println();
                    }
                    System.out.println();
                }
                case 2 -> { // buy a ticket
                    while (!ticketSelected) {
                        System.out.println("Enter a row number:");
                        row1 = scanner.nextInt();
                        System.out.println("Enter a seat number in that row:");
                        col1 = scanner.nextInt();
                        int r = row1;
                        int c = col1;
                        if (r > row || c > row) {
                            System.out.println();
                            System.out.println("Wrong input!");
                            System.out.println();
                        } else if (cinemas[r - 1][c - 1] == 'B') {
                            System.out.println();
                            System.out.println("That ticket has already been purchased!");
                            System.out.println();
                        } else {
                            ticketSelected = true;
                        }
                    }
                    ticketSelected = false;
                    cinemas[row1 - 1][col1 - 1] = 'B';
                    ticketPrice = (totalOfSeats < 60) ? seatPrice :
                            (row1 <= halfRow) ? seatPrice : seatPrice - 2;
                    System.out.println();
                    System.out.println("Ticket price: $" + ticketPrice);
                    System.out.println();
                    nPurchasedTicket++;
                    currentIncome += ticketPrice;
                }
                case 3 -> { // statistics
                    percentage = ((float) nPurchasedTicket / totalOfSeats) * 100;
                    totalIncome = totalOfSeats < 60 ? totalOfSeats * seatPrice :
                            row % 2 == 0 ? row / 2 * col * seatPrice + row / 2 * col * (seatPrice - 2) :
                                    (row - 1) / 2 * col * seatPrice + (row + 1) / 2 * col * (seatPrice - 2);
                    String statistics = """
                            Number of purchased tickets: %d
                            Percentage: %.2f%%
                            Current income: $%d
                            Total income: $%d""".formatted(nPurchasedTicket, percentage, currentIncome, totalIncome);
                    System.out.println(statistics);
                    System.out.println();
                }
                case 0 -> // exit
                        isStopped = true;
            }
        }
    }
}
