package xml;

import beans.DataTable.DataSheet;
import org.xml.sax.SAXException;

import javax.xml.XMLConstants;
import javax.xml.bind.JAXBContext;
import javax.xml.bind.JAXBException;
import javax.xml.bind.Unmarshaller;
import javax.xml.validation.Schema;
import javax.xml.validation.SchemaFactory;
import java.io.File;

public class LoadXML {

    public static DataSheet XmlToDataSheet(String filePath) throws JAXBException, SAXException {
        Unmarshaller unmarshaller = JAXBContext.newInstance(DataSheet.class).createUnmarshaller();
        SchemaFactory schemaFactory = SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);
        Schema schema = schemaFactory.newSchema(LoadXML.class.getResource("DataSheetXMLValidation.xsd"));
        unmarshaller.setSchema(schema);
        Object element = unmarshaller.unmarshal(new File(filePath));
        if (element instanceof DataSheet) {
            return (DataSheet) element;
        } else {
            throw new JAXBException("Error");
        }
    }
}
