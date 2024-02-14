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


------------

import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.pdmodel.PDPage;
import org.apache.pdfbox.pdmodel.PDPageContentStream;
import org.apache.pdfbox.pdmodel.common.PDRectangle;
import org.apache.pdfbox.pdmodel.graphics.color.PDColor;
import org.apache.pdfbox.pdmodel.graphics.color.PDDeviceRGB;

import java.awt.Color;
import java.io.IOException;

public class BlueRectangleCreator {

    public static void main(String[] args) {
        try (PDDocument document = new PDDocument()) {
            PDPage page = new PDPage(PDRectangle.A4);
            document.addPage(page);

            PDPageContentStream contentStream = new PDPageContentStream(document, page);
            
            // Set fill color to blue
            PDColor blue = new PDColor(new float[]{0, 0, 1}, PDDeviceRGB.INSTANCE);
            contentStream.setNonStrokingColor(blue);

            // Draw blue rectangle
            float pageWidth = page.getMediaBox().getWidth();
            float pageHeight = page.getMediaBox().getHeight();
            float rectangleHeight = 10;
            contentStream.addRect(0, pageHeight - rectangleHeight, pageWidth, rectangleHeight);
            contentStream.fill();

            contentStream.close();

            document.save("blue_rectangle.pdf");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
--------
import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.pdmodel.PDPage;
import org.apache.pdfbox.pdmodel.PDPageContentStream;
import org.apache.pdfbox.pdmodel.graphics.image.LosslessFactory;
import org.apache.pdfbox.pdmodel.graphics.image.PDImageXObject;
import org.apache.pdfbox.pdmodel.common.PDRectangle;

import java.io.IOException;
import java.io.InputStream;
import java.io.File;
import java.io.FileInputStream;

public class AddImageToPDF {

    public static void main(String[] args) {
        try (PDDocument document = new PDDocument()) {
            PDPage page = new PDPage(PDRectangle.A4);
            document.addPage(page);

            PDImageXObject image = loadImage(document, "path/to/image.png");

            try (PDPageContentStream contentStream = new PDPageContentStream(document, page)) {
                // Adjust the position and size of the image as needed
                contentStream.drawImage(image, 100, 500, image.getWidth() / 2, image.getHeight() / 2);
            }

            document.save("output.pdf");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static PDImageXObject loadImage(PDDocument document, String imagePath) throws IOException {
        try (InputStream in = new FileInputStream(new File(imagePath))) {
            return LosslessFactory.createFromImage(document, javax.imageio.ImageIO.read(in));
        }
    }
}
---------

import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.pdmodel.PDPage;
import org.apache.pdfbox.pdmodel.PDPageContentStream;
import org.apache.pdfbox.pdmodel.common.PDRectangle;
import org.apache.pdfbox.pdmodel.font.PDType1Font;

import java.io.IOException;

public class TableWithBordersPDF {

    public static void main(String[] args) {
        try (PDDocument document = new PDDocument()) {
            PDPage page = new PDPage(PDRectangle.A4);
            document.addPage(page);

            try (PDPageContentStream contentStream = new PDPageContentStream(document, page)) {
                drawTable(contentStream, page.getMediaBox().getWidth(), 700, new String[]{"Header 1", "Header 2", "Header 3"}, new String[][]{
                        {"Row 1 Col 1", "Row 1 Col 2", "Row 1 Col 3"},
                        {"Row 2 Col 1", "Row 2 Col 2", "Row 2 Col 3"}
                });
            }

            document.save("table_with_borders.pdf");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void drawTable(PDPageContentStream contentStream, float pageWidth, float y, String[] headers, String[][] rows) throws IOException {
        final int rowsCount = rows.length;
        final int colsCount = headers.length;
        final float rowHeight = 20f;
        final float tableWidth = pageWidth - 40; // Assuming 20 units margin on each side

        float cellMargin = 5f;
        float tableYPosition = y;
        float tableXPosition = 20; // Left margin

        // Draw headers
        float nextXStart = tableXPosition;
        float cellWidth = tableWidth / colsCount;
        for (String header : headers) {
            drawCell(contentStream, nextXStart, tableYPosition, cellWidth, rowHeight, header);
            nextXStart += cellWidth;
        }

        tableYPosition -= rowHeight;

        // Draw rows
        for (int i = 0; i < rowsCount; i++) {
            String[] row = rows[i];
            nextXStart = tableXPosition;
            for (String rowData : row) {
                drawCell(contentStream, nextXStart, tableYPosition, cellWidth, rowHeight, rowData);
                nextXStart += cellWidth;
            }
            tableYPosition -= rowHeight;
        }
    }

    private static void drawCell(PDPageContentStream contentStream, float xStart, float yStart, float width, float height, String content) throws IOException {
        contentStream.setLineWidth(0.5f);
        contentStream.moveTo(xStart, yStart);
        contentStream.lineTo(xStart + width, yStart);
        contentStream.stroke();

        contentStream.moveTo(xStart, yStart);
        contentStream.lineTo(xStart, yStart - height);
        contentStream.stroke();

        contentStream.moveTo(xStart + width, yStart);
        contentStream.lineTo(xStart + width, yStart - height);
        contentStream.stroke();

        contentStream.moveTo(xStart, yStart - height);
        contentStream.lineTo(xStart + width, yStart - height);
        contentStream.stroke();

        contentStream.beginText();
        contentStream.setFont(PDType1Font.HELVETICA, 12);
        contentStream.newLineAtOffset(xStart + 2, yStart - 12);
        contentStream.showText(content);
        contentStream.endText();
    }
}
-----
@PostMapping("/generate-pdf")
    public void generatePDF(@RequestBody String htmlContent) throws IOException {
        String pdfFilePath = "path/to/generated.pdf"; // Change this to your desired file path
        File pdfFile = new File(pdfFilePath);
        
        // Convert HTML content to PDF
        try (FileOutputStream fos = new FileOutputStream(pdfFile)) {
            ITextRenderer renderer = new ITextRenderer();
            renderer.setDocumentFromString(htmlContent);
            renderer.layout();
            renderer.createPDF(fos);
            fos.close();
        }
    }

----
