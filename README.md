import java.io.*;

public void removeLastRow(String inputFilePath, String tempFilePath) throws IOException {
    try (BufferedReader reader = new BufferedReader(new FileReader(inputFilePath));
         BufferedWriter writer = new BufferedWriter(new FileWriter(tempFilePath))) {
        
        String line;
        String previousLine = null;
        while ((line = reader.readLine()) != null) {
            if (previousLine != null) {
                writer.write(previousLine);
                writer.newLine();
            }
            previousLine = line;
        }
    }
    // Replace original file with temporary file
    new File(tempFilePath).renameTo(new File(inputFilePath));
}
