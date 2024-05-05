package beans.DataTable;

import javax.xml.bind.annotation.XmlAccessType;
import javax.xml.bind.annotation.XmlAccessorType;
import javax.xml.bind.annotation.XmlType;
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

@XmlAccessorType(XmlAccessType.FIELD)
@XmlType(name = "DataItem", propOrder = {
        "date",
        "x",
        "y"
})

public class DataItem {

    private Date date;
    private double x;
    private double y;

    public DataItem() {
        date = new Date();
        x = 0.0;
        y = 0.0;
    }

    public String getDate() {
        DateFormat formatter = new SimpleDateFormat("dd.MM.yyyy");
        return formatter.format(date);
    }

    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    public void setDate(String date) {
        DateFormat formatter = new SimpleDateFormat("dd.MM.yyyy");
        try {
            this.date = formatter.parse(date);
        } catch (ParseException error) {
            this.date = new Date();
        }
    }

    public void setX(double x) {
        this.x = x;
    }

    public void setY(double y) {
        this.y = y;
    }
}
