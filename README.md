# ECOcal



//code 

import java.util.Scanner;

 class Main {

    // Constants for Transportation
    private static final double PETROL_SEDAN_EMISSION = 143; // g CO2 per km
    private static final double DIESEL_SEDAN_EMISSION = 164; // g CO2 per km
    private static final double PETROL_SUV_EMISSION = 200; // g CO2 per km
    private static final double DIESEL_SUV_EMISSION = 240; // g CO2 per km
    private static final double PETROL_HYBRID_EMISSION = 100; // g CO2 per km
    private static final double DIESEL_HYBRID_EMISSION = 120; // g CO2 per km
    private static final double ELECTRIC_CAR_EMISSION = 0; // g CO2 per km (assumed zero for electric)
    private static final double MOTORCYCLE_EMISSION = 88; // g CO2 per km
    private static final double BUS_EMISSION = 104; // g CO2 per km

    // Constants for Electricity
    private static final double ELECTRICITY_EMISSION = 0.233; // kg CO2 per kWh

    // Constants for Average Carbon Footprint
    private static final double AVERAGE_CARBON_FOOTPRINT = 2000; // g CO2

    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        double totalCarbonFootprint = 0;

        // Get transportation details
        if (isTransportationUsed()) {
            String transportType = getTransportType();
            double distance = getDistance();
            double transportationCarbonFootprint = calculateTransportationCarbonFootprint(transportType, distance);
            System.out.printf("Your transportation carbon emissions are %.2f g CO2%n", transportationCarbonFootprint);
            totalCarbonFootprint += transportationCarbonFootprint;
        }

        // Get electricity usage details
        double electricityUsed = getElectricityUsed();
        double electricityCarbonFootprint = calculateElectricityCarbonFootprint(electricityUsed);
        System.out.printf("Your electricity carbon emissions are %.2f kg CO2%n", electricityCarbonFootprint);
        totalCarbonFootprint += electricityCarbonFootprint * 1000; // Convert kg to g

        // Display total carbon footprint
        System.out.printf("Your total carbon emissions are %.2f g CO2%n", totalCarbonFootprint);

        // Provide yearly estimates
        double yearlyFootprint = totalCarbonFootprint * getAnnualMultiplier();
        System.out.printf("Estimated yearly carbon emissions: %.2f g CO2%n", yearlyFootprint);

        // Compare with average and provide suggestions
        if (totalCarbonFootprint > AVERAGE_CARBON_FOOTPRINT) {
            System.out.println("Your carbon emissions are higher than the average.");
            printWaysToReduceCarbonEmissions();
        } else {
            System.out.println("Your carbon emissions are below the average.");
        }
    }

    private static boolean isTransportationUsed() {
        System.out.print("Did you use any transportation today? (yes/no): ");
        String transport = scanner.nextLine().toLowerCase();
        return transport.equals("yes");
    }

    private static String getTransportType() {
        System.out.print("What type of transport did you use? (car, motorcycle, bus): ");
        String transportType = scanner.nextLine().toLowerCase();
        if (transportType.equals("car")) {
            transportType = getCarType(); // Get specific car type
        }
        return transportType;
    }

    private static String getCarType() {
        System.out.print("What type of car is it? (petrol sedan, diesel sedan, petrol suv, diesel suv, petrol hybrid, diesel hybrid, electric): ");
        String carType = scanner.nextLine().toLowerCase();
        return carType;
    }

    private static double getDistance() {
        System.out.print("How many kilometers did you travel?: ");
        while (!scanner.hasNextDouble()) {
            System.out.println("Please enter a valid number for distance.");
            scanner.next(); // Clear invalid input
        }
        return scanner.nextDouble();
    }

    private static double getElectricityUsed() {
        System.out.print("How many kWh of electricity did you use? (e.g., 5 for 5 kWh): ");
        while (!scanner.hasNextDouble()) {
            System.out.println("Please enter a valid number for electricity usage.");
            scanner.next(); // Clear invalid input
        }
        return scanner.nextDouble();
    }

    private static double calculateTransportationCarbonFootprint(String transportType, double distance) {
        switch (transportType) {
            case "car":
                return calculateCarCarbonFootprint(getCarType(), distance);
            case "motorcycle":
                return distance * MOTORCYCLE_EMISSION;
            case "bus":
                return distance * BUS_EMISSION;
            default:
                System.out.println("Invalid transportation type.");
                return 0;
        }
    }

    private static double calculateCarCarbonFootprint(String carType, double distance) {
        switch (carType) {
            case "petrol sedan":
                return distance * PETROL_SEDAN_EMISSION;
            case "diesel sedan":
                return distance * DIESEL_SEDAN_EMISSION;
            case "petrol suv":
                return distance * PETROL_SUV_EMISSION;
            case "diesel suv":
                return distance * DIESEL_SUV_EMISSION;
            case "petrol hybrid":
                return distance * PETROL_HYBRID_EMISSION;
            case "diesel hybrid":
                return distance * DIESEL_HYBRID_EMISSION;
            case "electric":
                return distance * ELECTRIC_CAR_EMISSION;
            default:
                System.out.println("Invalid car type.");
                return 0;
        }
    }

    private static double calculateElectricityCarbonFootprint(double electricityUsed) {
        return electricityUsed * ELECTRICITY_EMISSION;
    }

    private static double getAnnualMultiplier() {
        System.out.print("Is this a daily or monthly estimate? (daily/monthly): ");
        String estimateType = scanner.nextLine().toLowerCase();
        switch (estimateType) {
            case "daily":
                return 365; // Daily estimate to yearly
            case "monthly":
                return 12; // Monthly estimate to yearly
            default:
                System.out.println("Invalid estimate type. Assuming daily.");
                return 365;
        }
    }

    private static void printWaysToReduceCarbonEmissions() {
        System.out.println("\nWays to reduce carbon emissions:");
        System.out.println("1. Switch to energy-efficient appliances and use LED bulbs.");
        System.out.println("2. Use public transportation, carpool, bike, or walk instead of driving.");
        System.out.println("3. Reduce meat consumption and choose local, seasonal foods.");
        System.out.println("4. Practice waste reduction, recycling, and composting.");
        System.out.println("5. Conserve water and use water-efficient fixtures.");
        System.out.println("6. Consider installing solar panels or improving home insulation.");
        System.out.println("7. Buy less and choose sustainable products.");
        System.out.println("8. Support and advocate for policies that address climate change.");
        System.out.println("9. Offset your carbon footprint by investing in carbon reduction projects.");
    }
}
