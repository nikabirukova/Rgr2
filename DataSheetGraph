package beans.DataGraph;

import beans.DataTable.DataSheet;

import javax.swing.*;
import java.awt.*;
import java.awt.geom.Line2D;
import java.awt.geom.Rectangle2D;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class DataSheetGraph extends JPanel {

    private static final long serialVersionUID = 1L;

    private DataSheet dataSheet;
    private boolean isConnected;
    private int deltaX;
    private int deltaY;
    private double xStep;
    private double yStep;
    transient private Color color;

    public DataSheetGraph() {
        super();
        isConnected = false;
        deltaX = 5;
        deltaY = 5;
        xStep = 1;
        yStep = 1;
        color = Color.red;
        setSize(500, 500);
    }

    public DataSheet getDataSheet() {
        return dataSheet;
    }

    public void setDataSheet(DataSheet dataSheet) {
        this.dataSheet = dataSheet;
    }

    public boolean isConnected() {
        return isConnected;
    }

    public void setConnected(boolean isConnected) {
        this.isConnected = isConnected;
        repaint();
    }

    public int getDeltaX() {
        return deltaX;
    }

    public int getDeltaY() {
        return deltaY;
    }

    public void setDeltaX(int deltaX) {
        this.deltaX = deltaX;
        repaint();
    }

    public void setDeltaY(int deltaY) {
        this.deltaY = deltaY;
        repaint();
    }

    public void setXStep(double xStep) {
        this.xStep = xStep;
    }

    public void setYStep(double yStep) {
        this.yStep = yStep;
    }

    public Color getColor() {
        return color;
    }

    public void setColor(Color color) {
        this.color = color;
        repaint();
    }

    private double minX() {
        double result = 0;
        if (dataSheet != null) {
            result = dataSheet.getDataItem(0).getX();
            for (int i = 0; i < dataSheet.size(); i++) {
                if (dataSheet.getDataItem(i).getX() < result) {
                    result = dataSheet.getDataItem(i).getX();
                }
            }
        }
        return result;
    }

    private double maxX() {
        double result = 0;
        if (dataSheet != null) {
            result = dataSheet.getDataItem(0).getX();
            for (int i = 0; i < dataSheet.size(); i++) {
                if (dataSheet.getDataItem(i).getX() > result) {
                    result = dataSheet.getDataItem(i).getX();
                }
            }
        }
        return result;
    }

    private double minY() {
        double result = 0;
        if (dataSheet != null) {
            result = dataSheet.getDataItem(0).getY();
            for (int i = 0; i < dataSheet.size(); i++) {
                if (dataSheet.getDataItem(i).getY() < result) {
                    result = dataSheet.getDataItem(i).getY();
                }
            }
        }
        return result;
    }

    private double maxY() {
        double result = 0;
        if (dataSheet != null) {
            result = dataSheet.getDataItem(0).getY();
            for (int i = 0; i < dataSheet.size(); i++) {
                if (dataSheet.getDataItem(i).getY() > result) {
                    result = dataSheet.getDataItem(i).getY();
                }
            }
        }
        return result;
    }

    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);
        Graphics2D g2 = (Graphics2D) g;
        showGraph(g2);
    }

    public void showGraph(Graphics2D gr) {

        recalculateDelta();
        double xMin = minX() - deltaX;
        double xMax = maxX() + deltaX;
        double yMin = minY() - deltaY;
        double yMax = maxY() + deltaY;
        double width = getWidth();
        double height = getHeight();

        // Определяем коэффициенты преобразования и положение начала координат
        double xScale = width / (xMax - xMin);
        double yScale = height / (yMax - yMin);
        double x0 = -xMin * xScale;
        double y0 = yMax * yScale;

        // Заполняем область графика белым цветом
        Paint oldColor = gr.getPaint();
        gr.setPaint(Color.WHITE);
        gr.fill(new Rectangle2D.Double(0.0, 0.0, width, height));
        Stroke oldStroke = gr.getStroke();
        Font oldFont = gr.getFont();

        // Настройка вида линий
        float[] dashPattern = {10, 10};
        gr.setStroke(new BasicStroke(1.0f, BasicStroke.CAP_BUTT, BasicStroke.JOIN_MITER, 10.0f, dashPattern, 0));
        gr.setFont(new Font("Arial", Font.BOLD, 14));

        recalculateSteps();
        // Сетка для оси X
        double xStep = this.xStep;
        for (double dx = xStep; dx < xMax; dx += xStep) {
            drawHorizontalLine(gr, xStep, height, x0, dx, xScale);
        }
        for (double dx = -xStep; dx >= xMin; dx -= xStep) {
            drawHorizontalLine(gr, xStep, height, x0, dx, xScale);
        }

        // Сетка для оси Y
        double yStep = this.yStep;
        for (double dy = yStep; dy < yMax; dy += yStep) {
            drawVerticalLine(gr, yStep, width, y0, dy, yScale);
        }
        for (double dy = -yStep; dy >= yMin; dy -= yStep) {
            drawVerticalLine(gr, yStep, width, y0, dy, yScale);
        }

        // Оси координат
        drawAxis(gr, x0, height, y0, width);

        // Отображаем точки, если определено хранилище
        if (dataSheet != null) {
            if (!isConnected) {
                for (int i = 0; i < dataSheet.size(); i++) {
                    double x = x0 + (dataSheet.getDataItem(i).getX() * xScale);
                    double y = y0 - (dataSheet.getDataItem(i).getY() * yScale);
                    gr.setColor(Color.white);
                    gr.fillOval((int) (x - 5 / 2), (int) (y - 5 / 2), 5, 5);
                    gr.setColor(color);
                    gr.drawOval((int) (x - 5 / 2), (int) (y - 5 / 2), 5, 5);
                }
            } else {
                gr.setPaint(color);
                gr.setStroke(new BasicStroke(2.0f));
                double xOld = x0 + dataSheet.getDataItem(0).getX() * xScale;
                double yOld = y0 - dataSheet.getDataItem(0).getY() * yScale;
                for (int i = 1; i < dataSheet.size(); i++) {
                    double x = x0 + dataSheet.getDataItem(i).getX() * xScale;
                    double y = y0 - dataSheet.getDataItem(i).getY() * yScale;
                    gr.draw(new Line2D.Double(xOld, yOld, x, y));
                    xOld = x;
                    yOld = y;
                }
            }
            // Восстанавливаем исходные значения
            gr.setPaint(oldColor);
            gr.setStroke(oldStroke);
            gr.setFont(oldFont);
        }
    }

    private void recalculateSteps() {
        double maxNumber = maxNumberFromPoints();
        int newStep;
        if (maxNumber != 0) {
            newStep = (int) Math.pow(10, (int) Math.log10(maxNumber));
        } else {
            newStep = 1;
        }
        setXStep(newStep);
        setYStep(newStep);
    }

    private void recalculateDelta() {
        int maxNumber = (int) maxNumberFromPoints();
        setDeltaX(maxNumber / 2 + 5);
        setDeltaY(maxNumber / 2 + 5);
    }

    private double maxNumberFromPoints() {
        Double[] maxNumbers = new Double[]{Math.abs(maxX()), Math.abs(minX()), Math.abs(maxY()), Math.abs(minY())};
        List<Double> list = Arrays.asList(maxNumbers);
        return Collections.max(list);
    }

    private void drawHorizontalLine(Graphics2D gr, double xStep, double height, double x0, double dx, double xScale) {
        double x = x0 + dx * xScale;
        gr.setPaint(Color.LIGHT_GRAY);
        gr.draw(new Line2D.Double(x, 0, x, height));
        gr.setPaint(Color.BLACK);
        gr.drawString(Math.round(dx / xStep) * xStep + "", (int) x + 2, 10);
    }

    private void drawVerticalLine(Graphics2D gr, double yStep, double width, double y0, double dy, double yScale) {
        double y = y0 - dy * yScale;
        gr.setPaint(Color.LIGHT_GRAY);
        gr.draw(new Line2D.Double(0, y, width, y));
        gr.setPaint(Color.BLACK);
        gr.drawString(Math.round(dy / yStep) * yStep + "", 2, (int) y - 2);
    }

    private void drawAxis(Graphics2D gr, double x0, double height, double y0, double width) {
        gr.setPaint(Color.BLACK);
        gr.setStroke(new BasicStroke(3.0f));
        gr.draw(new Line2D.Double(x0, 0, x0, height));
        gr.draw(new Line2D.Double(0, y0, width, y0));
        gr.drawString("X", (int) width - 10, (int) y0 - 2);
        gr.drawString("Y", (int) x0 + 2, 10);
    }
}
