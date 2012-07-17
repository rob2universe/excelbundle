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
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

import org.apache.poi.hssf.usermodel.HSSFCell;
import org.apache.poi.hssf.usermodel.HSSFCellStyle;
import org.apache.poi.hssf.usermodel.HSSFFont;
import org.apache.poi.hssf.usermodel.HSSFRichTextString;
import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.hssf.util.HSSFColor;

/**
 * Class for exporting LanguagePacks to Excel files.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class ExcelExporter
{
    // Attributes ----------------------------------------------------
    private Map<String, LanguagePack> langPacks =
    	new LinkedHashMap<String, LanguagePack>();
    private LanguagePack reference;
    private Map<String, String> sheetMap;
    
    private boolean redmark;
    private HSSFWorkbook wb;
	private HSSFCellStyle bundlePathStyle;
	private HSSFCellStyle languageStyle;
	private HSSFCellStyle keyStyle;
	private HSSFCellStyle valueStyle;
	private HSSFCellStyle missingStyle;
    
    // Public --------------------------------------------------------
	/**
	 * Sets the LanguagePack whoose keys are used as reference describing what
	 * key are actually supposed to be included in the resulting Excel file.
	 */
    public void setReferenceLanguage(LanguagePack reference)
    {
    	this.reference = reference;
    }
    
    /**
	 * Adds a LanguagePack to be exported.
	 */
	public void addLanguagePack(LanguagePack pack)
	{
		langPacks.put(pack.getLanguage(), pack);
	}
	
	/**
	 * Sets the sheet map. This Map is used to determine on what sheet a 
	 * language file will be output. The key is the path prefix and the value
	 * is the sheet name.
	 */
	public void setSheetMap(Map<String, String> sheetMap)
	{
		this.sheetMap = sheetMap;
	}
	
	/**
	 * Writes the result to a file.
	 */
	public void write(File file, boolean redmark) throws IOException
	{
		if(reference == null)
			throw new IllegalStateException(
					"No reference language pack specified yet");
		
		this.redmark = redmark;
		initialize();
		
		//If we don't have a sheetMap, write everything to the "All" sheet
		if(sheetMap == null)
			createSheet("All", reference);
		else
		{
			List<LanguageFile> all = new ArrayList<LanguageFile>(
					reference.getLanguageFiles());
			//Sort out all of the LanguageFiles that have a mapping and create 
			//sheets from them
			for(Map.Entry<String, String> mapping : sheetMap.entrySet())
			{
				String pathPrefix = mapping.getKey();
				LanguagePack pack = new LanguagePack(reference.getLanguage());
				Iterator<LanguageFile> it = all.iterator();
				while(it.hasNext())
				{
					LanguageFile langFile = it.next();
					if(pathPrefix.length() > langFile.getPath().length())
						continue;
					
					if(langFile.getPath().substring(0, pathPrefix.length())
							.equals(pathPrefix))
					{
						pack.addLanguageFile(langFile);
						it.remove();
					}
				}
				
				createSheet(mapping.getValue(), pack);
			}
			
			//If there's anything left, add it to a misc sheet
			if(!all.isEmpty())
			{
				LanguagePack pack = new LanguagePack(reference.getLanguage());
				for(LanguageFile langFile : all)
					pack.addLanguageFile(langFile);
				createSheet("Misc", pack);
			}
		}
		
		wb.write(new FileOutputStream(file));
	}
	
    // Private -------------------------------------------------------
	/**
	 * Do stuff like creating the workbook and creating the styles.
	 */
	private void initialize()
	{
		wb = new HSSFWorkbook();
		
		//Create style for bundle path cells
		bundlePathStyle = wb.createCellStyle();
		HSSFFont font = wb.createFont();
		font.setFontHeightInPoints((short)14);
		font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
		bundlePathStyle.setFont(font);
		bundlePathStyle.setLocked(true);
		
		//Create style for language column
		languageStyle = wb.createCellStyle();
		font = wb.createFont();
		font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
		languageStyle.setFont(font);
		languageStyle.setBorderRight(HSSFCellStyle.BORDER_THIN);
		languageStyle.setBorderLeft(HSSFCellStyle.BORDER_THIN);
		languageStyle.setBorderTop(HSSFCellStyle.BORDER_THIN);
		languageStyle.setBorderBottom(HSSFCellStyle.BORDER_MEDIUM);
		languageStyle.setLocked(true);
		
		//Create style for key cells
		keyStyle = wb.createCellStyle();
		font = wb.createFont();
		font.setBoldweight(HSSFFont.BOLDWEIGHT_BOLD);
		keyStyle.setFont(font);
		keyStyle.setBorderTop(HSSFCellStyle.BORDER_THIN);
		keyStyle.setBorderBottom(HSSFCellStyle.BORDER_THIN);
		keyStyle.setBorderTop(HSSFCellStyle.BORDER_THIN);
		keyStyle.setBorderRight(HSSFCellStyle.BORDER_MEDIUM);
		keyStyle.setLocked(true);
		
		//Create style for value cells
		valueStyle = wb.createCellStyle();
		valueStyle.setWrapText(true);
		valueStyle.setBorderBottom(HSSFCellStyle.BORDER_THIN);
		valueStyle.setBorderTop(HSSFCellStyle.BORDER_THIN);
		valueStyle.setBorderLeft(HSSFCellStyle.BORDER_THIN);
		valueStyle.setBorderRight(HSSFCellStyle.BORDER_THIN);
		valueStyle.setLocked(false);
		
		if(redmark)
		{
			//Create style for cells where values are missing
			missingStyle = wb.createCellStyle();
			missingStyle.setFillPattern(HSSFCellStyle.SOLID_FOREGROUND);
			missingStyle.setFillForegroundColor(HSSFColor.ORANGE.index);
			missingStyle.setBorderBottom(HSSFCellStyle.BORDER_THIN);
			missingStyle.setBorderTop(HSSFCellStyle.BORDER_THIN);
			missingStyle.setBorderLeft(HSSFCellStyle.BORDER_THIN);
			missingStyle.setBorderRight(HSSFCellStyle.BORDER_THIN);
			missingStyle.setLocked(false);
		}
	}
	
	/**
	 * Create a sheet and fill it with stuff.
	 */
	private void createSheet(String sheetName, LanguagePack referencePack)
	{
		HSSFSheet sheet = wb.createSheet(sheetName);
		sheet.setProtect(true);
		sheet.setDisplayGridlines(false);
		sheet.createFreezePane( 1, 0, 1, 0 );
		
		//Set some column widths:
		int numCols = langPacks.size() + 2;
		for(short i = 0; i <= numCols; i++)
			sheet.setColumnWidth(i, (short)10000);
		
		//Now lets fill it with stuff
		HSSFRow r;
		HSSFCell c;
		int rowIndex = 0;
		short colIndex;
		for(LanguageFile refLangFile : referencePack.getLanguageFiles())
		{
			//Bundle path
			r = sheet.createRow(rowIndex);
			c = r.createCell((short)0);
			c.setCellStyle(bundlePathStyle);
			c.setCellValue(new HSSFRichTextString(refLangFile.getPath()));
			rowIndex++;
			
			//"Table header"
			r = sheet.createRow(rowIndex);
			colIndex = 1;
			for(LanguagePack pack : langPacks.values())
			{
				c = r.createCell(colIndex);
				c.setCellStyle(languageStyle);
				c.setCellValue(new HSSFRichTextString(pack.getLanguage()));
				colIndex++;
			}
			rowIndex++;
			
			//Keys and values
			for(LanguageFile.KeyValuePair pair: refLangFile.getPairs())
			{
				r = sheet.createRow(rowIndex);
				r.setHeightInPoints(16);
				c = r.createCell((short)0);
				c.setCellStyle(keyStyle);
				c.setCellValue(new HSSFRichTextString(pair.getKey()));
				
				colIndex = 1;
				for(LanguagePack pack : langPacks.values())
				{
					c = r.createCell(colIndex);
					
					String value = null;
					LanguageFile langFile = 
						pack.getLanguageFile(refLangFile.getPath());
					if(langFile != null)
						value = langFile.getValue(pair.getKey());
						
					if(value == null)
					{
						if(redmark)
							c.setCellStyle(missingStyle);
						else
							c.setCellStyle(valueStyle);
					}
					else
					{
						c.setCellStyle(valueStyle);
						c.setCellValue(new HSSFRichTextString(value));
					}
					colIndex++;
				}
				rowIndex++;
			}
			rowIndex++;
		} //End of sheet creation
	}
}