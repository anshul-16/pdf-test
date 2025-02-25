import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

import java.time.LocalDate;
import java.time.Month;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.MockitoAnnotations;

class DateCalculatorTest {

    @InjectMocks
    private YourClassName instance; // Replace with the actual class name

    @Mock
    private YourClassName mockInstance; // If helper methods are in the same class, mock it

    private LocalDate curDate;

    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
        curDate = LocalDate.of(2025, Month.MARCH, 15); // Example current date
    }

    @Test
    void testAnnualFrequency() {
        DateRange result = instance.dateCalculator(curDate, "ANNUALLY");

        assertNotNull(result);
        assertEquals(LocalDate.of(2025, Month.JANUARY, 1), result.getStartDate());
        assertEquals(LocalDate.of(2025, Month.DECEMBER, 31), result.getEndDate());
    }

    @Test
    void testSemiAnnualFrequency() {
        when(mockInstance.getSemiAnnualStartDate(curDate)).thenReturn(LocalDate.of(2025, Month.JANUARY, 1));

        DateRange result = instance.dateCalculator(curDate, "SEMI_ANNUALLY");

        assertNotNull(result);
        assertEquals(LocalDate.of(2025, Month.JANUARY, 1), result.getStartDate());
        assertEquals(LocalDate.of(2025, Month.JUNE, 30), result.getEndDate());
    }

    @Test
    void testTriAnnualFrequency() {
        when(mockInstance.getTriAnnualStartDate(curDate)).thenReturn(LocalDate.of(2025, Month.JANUARY, 1));

        DateRange result = instance.dateCalculator(curDate, "TRI_ANNUALLY");

        assertNotNull(result);
        assertEquals(LocalDate.of(2025, Month.JANUARY, 1), result.getStartDate());
        assertEquals(LocalDate.of(2025, Month.APRIL, 30), result.getEndDate());
    }

    @Test
    void testQuarterlyFrequency() {
        when(mockInstance.getQuarterStartDate(curDate)).thenReturn(LocalDate.of(2025, Month.JANUARY, 1));

        DateRange result = instance.dateCalculator(curDate, "QUARTERLY");

        assertNotNull(result);
        assertEquals(LocalDate.of(2025, Month.JANUARY, 1), result.getStartDate());
        assertEquals(LocalDate.of(2025, Month.MARCH, 31), result.getEndDate());
    }

    @Test
    void testInvalidFrequency() {
        DateRange result = instance.dateCalculator(curDate, "UNKNOWN");

        assertNotNull(result);
        assertNull(result.getStartDate());
        assertNull(result.getEndDate());
    }
}
