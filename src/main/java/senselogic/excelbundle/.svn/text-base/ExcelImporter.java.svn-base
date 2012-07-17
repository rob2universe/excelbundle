/*
 * Copyright 2006 Senselogic
 *
 *  Licensed under the Apache License, Version 2.0 (the "License");
 *  you may not use this file except in compliance with the License.
 *  You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 *  Unless required by applicable law or agreed to in writing, software
 *  distributed under the License is distributed on an "AS IS" BASIS,
 *  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 *  See the License for the specific language governing permissions and
 *  limitations under the License.
 */

package senselogic.excelbundle;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.ListIterator;

import org.apache.poi.hssf.usermodel.HSSFCell;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;


/**
 * Class for importing LanguagePacks from Excel files.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class ExcelImporter
{

    // Attributes ----------------------------------------------------
    private HSSFWorkbook wb;
    
    // Constructors --------------------------------------------------
    /**
     * Constructs a new ExcelImporter.
     * 
     * @param file  the file to load from
     */
    public ExcelImporter(File file) throws IOException
    {
    	wb = new HSSFWorkbook(new FileInputStream(file));
    }
    
    // Public --------------------------------------------------------
    /**
     * Loads the specified language and returns it as a LanguagePack.
     */
    public LanguagePack loadLanguage(String language)
    {
    	LanguagePack pack = new LanguagePack(language);
    	for(short i = 0; i < wb.getNumberOfSheets(); i++)
    		getFromSheet(wb.getSheetAt(i), language, pack);
    	
    	if(!pack.getLanguageFiles().isEmpty())
    		return pack;
    	
    	return null;
    }

    // Private -------------------------------------------------------
    /**
     * Fetch all LanguageFiles in the specified language from the specified
     * sheet and add them to the specified LanguagePack.
     */
    public void getFromSheet(HSSFSheet sheet, String language,
    		LanguagePack pack)
    {
    	//First, let's fetch all of the defined rows
    	List<HSSFRow> rows = new ArrayList<HSSFRow>();
    	int lastRow = sheet.getLastRowNum();
    	for(int i = sheet.getFirstRowNum(); i <= lastRow; i++)
    	{
    		//Don't add empty rows
    		HSSFRow r = sheet.getRow(i);
    		if((r != null) && (r.getPhysicalNumberOfCells() != 0))
    		{
    			rows.add(r);
    		}
    	}
    	
    	//Now go, through them one at a time
    	ListIterator<HSSFRow> it = rows.listIterator();
    	while(it.hasNext())
    	{
    		HSSFRow r = it.next();
    		HSSFCell c = r.getCell((short)0);
    		
    		String value = getString(c);
    		if((value.charAt(0) != '/') && (value.charAt(0) != '\\'))
    			continue;
    		
    		String bundlePath = value;
    		
    		//Let's see if this language is included
    		if(!it.hasNext())
    			break;
    		r = it.next();
    		short valueCol = getIndexOf(r, language);
    		//Skip to next if the language isn't included in this bundle
    		if(valueCol < 0)
    			continue;
    		
    		LanguageFile langFile = new LanguageFile(bundlePath, language);
    		//Now, search through all the keys
    		while(it.hasNext())
    		{
    			r = it.next();
    			String firstCell = getString(r.getCell((short)0));
    			if(firstCell == null)
    				continue;
    			
    			//If this is a bundle path, rewind and break the loop to parse 
    			//the next bundle
    			if( (firstCell.charAt(0) == '/') || 
    					(firstCell.charAt(0) == '\\') )
    			{
    				it.previous();
    				break;
    			}
    			
    			langFile.setValue(firstCell, getString(r.getCell(valueCol)));
    		}
    		if(!langFile.getPairs().isEmpty())
    			pack.addLanguageFile(langFile);
    	}
    }
    
    /**
     * Returns the trimmed string value of the cell of null if none can be 
     * created. Null will also be returned for empty strings or if the null is
     * passed as a value.
     */
	private String getString(HSSFCell c)
	{
		if(c == null)
			return null;
		if(c.getCellType() != HSSFCell.CELL_TYPE_STRING)
			return null;
		
		String str = c.getRichStringCellValue().getString();
		if(str.length() == 0)
			return null;
		
		return str;
	}
	
	/**
	 * Returns the first index of the specified value on the row or -1 if there
	 * is no such value.
	 */
	private short getIndexOf(HSSFRow r, String value)
	{
		short lastCell = r.getLastCellNum();
		for(short i = 1; i <= lastCell; i++)
		{
			String rowValue = getString(r.getCell(i));
			if(value.equals(rowValue))
				return i;
		}
		return -1;
	}
}