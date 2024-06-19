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
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 50px;
        }
        h1 {
            color: #333;
        }
        img {
            width: 200px;
            height: auto;
            margin-bottom: 20px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
    </style>
</head>
<body>
    <h1>Document Title</h1>
    <img src="https://via.placeholder.com/200" alt="Placeholder Image">
    <p>This is a sample document containing an image, a table, and other elements.</p>
    <table>
        <thead>
            <tr>
                <th>Column 1</th>
                <th>Column 2</th>
                <th>Column 3</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>Data 1</td>
                <td>Data 2</td>
                <td>Data 3</td>
            </tr>
            <tr>
                <td>Data 4</td>
                <td>Data 5</td>
                <td>Data 6</td>
            </tr>
        </tbody>
    </table>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed non risus.</p>
    <p>Nulla faucibus odio quis purus mollis, non volutpat mi hendrerit. Sed ac felis sit amet mi posuere efficitur.</p>
</body>
</html>

--------
import java.io.IOException;
import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.pdmodel.PDPage;
import org.apache.pdfbox.pdmodel.PDPageContentStream;
import org.apache.pdfbox.pdmodel.font.PDType1Font;
import org.apache.pdfbox.pdmodel.common.PDRectangle;

public class TableWithBorders {
    public static void main(String[] args) {
        try (PDDocument document = new PDDocument()) {
            PDPage page = new PDPage(PDRectangle.A4);
            document.addPage(page);

            PDPageContentStream contentStream = new PDPageContentStream(document, page);
            float margin = 50;
            float yStart = page.getMediaBox().getHeight() - margin;
            float tableWidth = page.getMediaBox().getWidth() - 2 * margin;
            float yPosition = yStart;
            int rows = 5;
            int cols = 3;
            float rowHeight = 20f;
            float tableMargin = 10f;
            float cellMargin = 2f;

            // Content
            String[][] content = {
                {"Header 1",
            "Header 2",
            "Header 3"},
            {"Row 1 Col 1", "Row 1 Col 2", "Row 1 Col 3"},
            {"Row 2 Col 1", "Row 2 Col 2", "Row 2 Col 3"},
            {"Row 3 Col 1", "Row 3 Col 2", "Row 3 Col 3"},
            {"Row 4 Col 1", "Row 4 Col 2", "Row 4 Col 3"}
        };

        float[][] columnWidths = new float[cols][rows];
        for (int i = 0; i < cols; i++) {
            float maxWidth = 0;
            for (int j = 0; j < rows; j++) {
                float width = PDType1Font.HELVETICA.getStringWidth(content[j][i]) / 1000 * 12;
                columnWidths[i][j] = width;
                if (width > maxWidth) {
                    maxWidth = width;
                }
            }
            columnWidths[i][rows] = maxWidth + 2 * cellMargin;
        }

        // Drawing table
        for (int i = 0; i <= rows; i++) {
            contentStream.drawLine(margin, yPosition, margin + tableWidth, yPosition);
            yPosition -= rowHeight;
        }
        for (int i = 0; i <= cols; i++) {
            float xPosition = margin;
            for (int j = 0; j < i; j++) {
                xPosition += columnWidths[j][rows];
            }
            contentStream.drawLine(xPosition, yStart, xPosition, yPosition + rowHeight);
        }
        yPosition = yStart - rowHeight;
        for (int i = 0; i < rows; i++) {
            float xPosition = margin;
            for (int j = 0; j < cols; j++) {
                contentStream.beginText();
                contentStream.setFont(PDType1Font.HELVETICA, 12);
                contentStream.newLineAtOffset(xPosition + cellMargin, yPosition - cellMargin);
                contentStream.showText(content[i][j]);
                contentStream.endText();
                xPosition += columnWidths[j][rows];
            }
            yPosition -= rowHeight;
        }

        contentStream.close();
        document.save("TableWithBorders.pdf");
        System.out.println("Table created successfully!");
    } catch (IOException e) {
        e.printStackTrace();
    }
}
----
import org.springframework.core.io.Resource;
import org.springframework.http.HttpHeaders;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

@Controller
public class FileController {

    private static final String FILE_DIRECTORY = "/path/to/your/files/directory/";

    @GetMapping("/files/{fileName}")
    public ResponseEntity<Resource> downloadFile(@PathVariable String fileName) {
        Path filePath = Paths.get(FILE_DIRECTORY, fileName);
        Resource resource;
        try {
            resource = new org.springframework.core.io.FileUrlResource(filePath.toUri());
        } catch (IOException e) {
            // Handle file not found exception
            return ResponseEntity.notFound().build();
        }

        // Try to determine file's content type
        String contentType;
        try {
            contentType = Files.probeContentType(filePath);
        } catch (IOException e) {
            contentType = MediaType.APPLICATION_OCTET_STREAM_VALUE;
        }

        // Fallback to application/octet-stream if content type could not be determined
        if (contentType == null) {
            contentType = MediaType.APPLICATION_OCTET_STREAM_VALUE;
        }

        return ResponseEntity.ok()
                .contentType(MediaType.parseMediaType(contentType))
                .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + resource.getFilename() + "\"")
                .body(resource);
    }
}
-----
import org.springframework.core.io.FileSystemResource;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.ArrayList;
import java.util.List;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

@RestController
@RequestMapping("/files")
public class FileController {

    @GetMapping("/download")
    public ResponseEntity<FileSystemResource> downloadFiles() throws IOException {
        String directoryPath = "/path/to/your/directory"; // Specify your directory path here
        File directory = new File(directoryPath);

        if (!directory.exists() || !directory.isDirectory()) {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).body(null);
        }

        // Get list of files in the directory
        File[] files = directory.listFiles();
        List<Path> filePaths = new ArrayList<>();

        // Add file paths to the list
        for (File file : files) {
            if (file.isFile()) {
                filePaths.add(file.toPath());
            }
        }

        // Create a zip archive containing all files
        Path zipFilePath = Paths.get(directoryPath, "files.zip");
        try (ZipOutputStream zipOutputStream = new ZipOutputStream(Files.newOutputStream(zipFilePath))) {
            for (Path filePath : filePaths) {
                ZipEntry zipEntry = new ZipEntry(directory.toPath().relativize(filePath).toString());
                zipOutputStream.putNextEntry(zipEntry);
                Files.copy(filePath, zipOutputStream);
                zipOutputStream.closeEntry();
            }
        }

        // Send the zip file in the response
        FileSystemResource resource = new FileSystemResource(zipFilePath.toFile());
        HttpHeaders headers = new HttpHeaders();
        headers.add(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=files.zip");

        return ResponseEntity.ok()
                .headers(headers)
                .contentType(MediaType.APPLICATION_OCTET_STREAM)
                .body(resource);
    }
}
-------

/ Draw underline
            float textWidth = PDType1Font.HELVETICA_BOLD.getStringWidth(text) / 1000 * 12;
            float lineWidth = 1.5f;
            contentStream.setLineWidth(lineWidth);
            contentStream.moveTo(x, y - 3); // Move to start of underline
            contentStream.lineTo(x + textWidth, y - 3); // Draw line
            contentStream.closeAndStroke(); // Close the path and stroke it

            contentStream.close(); // Close the content stream
----
private byte[] inputStreamToByteArray(InputStream inputStream) throws IOException {
        ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
        byte[] buffer = new byte[4096];
        int bytesRead;
        while ((bytesRead = inputStream.read(buffer)) != -1) {
            outputStream.write(buffer, 0, bytesRead);
        }
        return outputStream.toByteArray();
    }
    -----
// Convert byte[] to InputStream
        InputStream inputStream = new ByteArrayInputStream(pdfBytes);

        // Set headers
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_PDF);
        headers.setContentDispositionFormData("filename", "document.pdf");

        // Create InputStreamResource
        InputStreamResource inputStreamResource = new InputStreamResource(inputStream);

        // Return response entity
        return ResponseEntity.ok()
                .headers(headers)
                .body(inputStreamResource);
    }
----
String base64EncodedPDF = Base64.getEncoder().encodeToString(pdfBytes);

        // Set headers
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_JSON);

        // Return the base64 encoded PDF as JSON response
        return ResponseEntity.ok()
                .headers(headers)
                .body("{\"pdf\": \"" + base64EncodedPDF + "\"}");
---
String input = "stringRSF-23-0000007SRSF-23-0000008String";

        String regex = "RSF-\\d{2}-\\d{7}";

        List<String> matches = extractMatches(input, regex);

        System.out.println("Matches:");
        for (String match : matches) {
            System.out.println(match);
        }
        ---
------------------------------

style="width: 100%; height: 100%;"
      [columnDefs]="columnDefs"
      [autoGroupColumnDef]="autoGroupColumnDef"
      [defaultColDef]="defaultColDef"
      [suppressRowClickSelection]="true"
      [groupSelectsChildren]="true"
      [rowSelection]="rowSelection"
      [rowGroupPanelShow]="rowGroupPanelShow"
      [pivotPanelShow]="pivotPanelShow"
      [pagination]="true"
      [paginationPageSize]="paginationPageSize"
      [paginationPageSizeSelector]="paginationPageSizeSelector"
      [paginationNumberFormatter]="paginationNumberFormatter"
      [rowData]="rowData"
      [class]="themeClass"
      (firstDataRendered)="onFirstDataRendered($event)"
      (gridReady)="onGridReady($event)"
--------------------

public paginationNumberFormatter: (
    params: PaginationNumberFormatterParams,
  ) => string = (params: PaginationNumberFormatterParams) => {
    return "[" + params.value.toLocaleString() + "]";
  };






----------
def split_sql_file(input_file, lines_per_file=50000):
    file_count = 0
    line_count = 0
    output_file = None

    with open(input_file, 'r', encoding='utf-8') as infile:
        for line in infile:
            if line_count % lines_per_file == 0:
                if output_file:
                    output_file.close()
                output_file = open(f'output_{file_count}.sql', 'w', encoding='utf-8')
                file_count += 1
            output_file.write(line)
            line_count += 1

    if output_file:
        output_file.close()

# Usage
split_sql_file('large_file.sql')
