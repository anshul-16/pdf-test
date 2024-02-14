# pdf-test
test pdf repo

import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.pdmodel.PDPage;
import org.apache.pdfbox.pdmodel.PDPageContentStream;
import org.apache.pdfbox.pdmodel.font.PDType1Font;

import java.io.IOException;

public class PdfTableCreator {

    public static void main(String[] args) {
        try (PDDocument document = new PDDocument()) {
            PDPage page = new PDPage();
            document.addPage(page);

            PDPageContentStream contentStream = new PDPageContentStream(document, page);

            // Define table parameters
            float margin = 50;
            float yStart = page.getMediaBox().getHeight() - margin;
            float tableWidth = page.getMediaBox().getWidth() - 2 * margin;
            float cellHeight = 20f;
            float rowHeight = 30f;
            float tableYPosition = yStart;
            int numberOfColumns = 3;

            // Define column widths
            float[] columnWidths = { tableWidth / 4, tableWidth / 4, tableWidth / 2 };

            // Draw table headers
            drawTableHeader(contentStream, margin, yStart, tableWidth, cellHeight, columnWidths);

            // Draw table rows
            drawTableRow(contentStream, margin, tableYPosition, tableWidth, rowHeight, columnWidths, "Column 1", "Column 2", "Column 3");
            tableYPosition -= rowHeight;
            drawTableRow(contentStream, margin, tableYPosition, tableWidth, rowHeight, columnWidths, "Row 1", "Row 1", "Row 1");
            tableYPosition -= rowHeight;
            drawTableRow(contentStream, margin, tableYPosition, tableWidth, rowHeight, columnWidths, "Row 2", "Row 2", "Row 2");

            contentStream.close();

            document.save("table.pdf");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void drawTableHeader(PDPageContentStream contentStream, float xStart, float yStart,
                                         float tableWidth, float cellHeight, float[] columnWidths) throws IOException {
        float nextXStart = xStart;
        for (float width : columnWidths) {
            contentStream.setFont(PDType1Font.HELVETICA_BOLD, 12);
            contentStream.beginText();
            contentStream.newLineAtOffset(nextXStart + 5, yStart - cellHeight / 2);
            contentStream.showText("Header");
            contentStream.endText();
            nextXStart += width;
        }
    }

    private static void drawTableRow(PDPageContentStream contentStream, float xStart, float yStart,
                                     float tableWidth, float rowHeight, float[] columnWidths, String... rowData) throws IOException {
        float nextXStart = xStart;
        for (int i = 0; i < columnWidths.length; i++) {
            float width = columnWidths[i];
            contentStream.setFont(PDType1Font.HELVETICA, 10);
            contentStream.setLineWidth(0.5f);
            contentStream.moveTo(nextXStart, yStart);
            contentStream.lineTo(nextXStart + width, yStart);
            contentStream.stroke();
            contentStream.beginText();
            contentStream.newLineAtOffset(nextXStart + 5, yStart - rowHeight / 2);
            contentStream.showText(rowData[i]);
            contentStream.endText();
            nextXStart += width;
        }
    }
}
