import java.time.LocalDate;
import java.time.Month;
import java.time.temporal.TemporalAdjusters;

public class InvoicePeriodCalculator {
    
    public static void main(String[] args) {
        LocalDate currentDate = LocalDate.now();
        
        System.out.println("Quarterly: " + getPeriod(currentDate, "Quarterly"));
        System.out.println("Semi-Annually: " + getPeriod(currentDate, "Semi-Annually"));
        System.out.println("Tri-Annually: " + getPeriod(currentDate, "Tri-Annually"));
        System.out.println("Annually: " + getPeriod(currentDate, "Annually"));
    }

    public static String getPeriod(LocalDate currentDate, String invoicingType) {
        LocalDate startDate, endDate;
        
        switch (invoicingType) {
            case "Quarterly":
                startDate = getQuarterStartDate(currentDate);
                endDate = startDate.plusMonths(3).minusDays(1);
                break;
                
            case "Semi-Annually":
                startDate = getSemiAnnualStartDate(currentDate);
                endDate = startDate.plusMonths(6).minusDays(1);
                break;
                
            case "Tri-Annually":
                startDate = getTriAnnualStartDate(currentDate);
                endDate = startDate.plusMonths(4).minusDays(1);
                break;
                
            case "Annually":
                startDate = LocalDate.of(currentDate.getYear(), Month.JANUARY, 1);
                endDate = LocalDate.of(currentDate.getYear(), Month.DECEMBER, 31);
                break;
                
            default:
                throw new IllegalArgumentException("Invalid invoicing type: " + invoicingType);
        }

        return "Start: " + startDate + ", End: " + endDate;
    }
    
    private static LocalDate getQuarterStartDate(LocalDate date) {
        int month = date.getMonthValue();
        int quarterStartMonth = ((month - 1) / 3) * 3 + 1;
        return LocalDate.of(date.getYear(), quarterStartMonth, 1);
    }

    private static LocalDate getSemiAnnualStartDate(LocalDate date) {
        int month = date.getMonthValue();
        int semiAnnualStartMonth = (month <= 6) ? 1 : 7;
        return LocalDate.of(date.getYear(), semiAnnualStartMonth, 1);
    }

    private static LocalDate getTriAnnualStartDate(LocalDate date) {
        int month = date.getMonthValue();
        int triAnnualStartMonth = (month <= 4) ? 1 : (month <= 8) ? 5 : 9;
        return LocalDate.of(date.getYear(), triAnnualStartMonth, 1);
    }
}
