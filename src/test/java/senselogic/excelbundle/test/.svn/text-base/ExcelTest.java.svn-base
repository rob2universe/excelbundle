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

package senselogic.excelbundle.test;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

import org.apache.poi.hssf.usermodel.HSSFRow;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;

import senselogic.excelbundle.ExcelExporter;
import senselogic.excelbundle.ExcelImporter;
import senselogic.excelbundle.LanguageFile;
import senselogic.excelbundle.LanguagePack;
import junit.framework.TestCase;

/**
 * TestCase for exporting and importing LanguagePacks to and form Excel files.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class ExcelTest extends TestCase
{
	File exportFile = new File("test/classes/test.xls");
	private HSSFSheet sheet;
	
	LanguagePack ref = new LanguagePack("en");
	LanguagePack sv = new LanguagePack("sv");
	String bundle1 = "/bundle1";
	String bundle2 = "/bundle2";
	String key1 = "key1";
	String key2 = "key2";
	String key3 = "key3";
	String key4 = "key4";
	
	protected void setUp() throws Exception
	{
		LanguageFile file1 = new LanguageFile(bundle1, "en");
		file1.setValue(key1, "en1");
		file1.setValue(key2, "en2");
		ref.addLanguageFile(file1);
		LanguageFile file2 = new LanguageFile(bundle2, "en");
		file2.setValue(key3, "en3");
		file2.setValue(key4, "en4");
		ref.addLanguageFile(file2);
		
		file1 = new LanguageFile(bundle1, "sv");
		file1.setValue(key1, "sv1");
		file1.setValue(key2, "sv2");
		sv.addLanguageFile(file1);
		file2 = new LanguageFile(bundle2, "sv");
		file2.setValue(key3, "sv3");
		file2.setValue(key4, "sv4");
		sv.addLanguageFile(file2);
		
		ExcelExporter ex = new ExcelExporter();
		ex.setReferenceLanguage(ref);
		ex.addLanguagePack(ref);
		ex.addLanguagePack(sv);
		ex.write(exportFile, true);
		
		//Now let's see if it's okay
		HSSFWorkbook wb = new HSSFWorkbook(new FileInputStream(exportFile));
		sheet = wb.getSheetAt(0);
	}
	
	public void testValues()
	{
		HSSFRow r = sheet.getRow(0);
		assertEquals(
				bundle1,
				r.getCell((short)0).getRichStringCellValue().getString());
		r = sheet.getRow(1);
		assertEquals(
				"en",
				r.getCell((short)1).getRichStringCellValue().getString());
		assertEquals(
				"sv",
				r.getCell((short)2).getRichStringCellValue().getString());
		r = sheet.getRow(2);
		assertEquals(
				"en1",
				r.getCell((short)1).getRichStringCellValue().getString());
		assertEquals(
				"sv1",
				r.getCell((short)2).getRichStringCellValue().getString());
		r = sheet.getRow(3);
		assertEquals(
				"en2",
				r.getCell((short)1).getRichStringCellValue().getString());
		assertEquals(
				"sv2",
				r.getCell((short)2).getRichStringCellValue().getString());
		r = sheet.getRow(5);
		assertEquals(
				bundle2,
				r.getCell((short)0).getRichStringCellValue().getString());
		r = sheet.getRow(6);
		assertEquals(
				"en",
				r.getCell((short)1).getRichStringCellValue().getString());
		assertEquals(
				"sv",
				r.getCell((short)2).getRichStringCellValue().getString());
		r = sheet.getRow(7);
		assertEquals(
				"en3",
				r.getCell((short)1).getRichStringCellValue().getString());
		assertEquals(
				"sv3",
				r.getCell((short)2).getRichStringCellValue().getString());
		r = sheet.getRow(8);
		assertEquals(
				"en4",
				r.getCell((short)1).getRichStringCellValue().getString());
		assertEquals(
				"sv4",
				r.getCell((short)2).getRichStringCellValue().getString());
	}
	
	public void testExportImport() throws IOException
	{
		File excelFile = new File("src/test/test2.xls");
		try
		{
			ExcelExporter ex = new ExcelExporter();
			ex.setReferenceLanguage(ref);
			ex.addLanguagePack(ref);
			ex.addLanguagePack(sv);
			ex.write(excelFile, true);
			
			ExcelImporter im = new ExcelImporter(excelFile);
			LanguagePack refLoad = im.loadLanguage("en");
			LanguagePack svLoad = im.loadLanguage("sv");
			
			//Compare written with loaded
			for(LanguageFile langFile : ref.getLanguageFiles())
				assertFalse(langFile.differs(refLoad.getLanguageFile(langFile.getPath())));
			for(LanguageFile langFile : sv.getLanguageFiles())
				assertFalse(langFile.differs(svLoad.getLanguageFile(langFile.getPath())));
		}
		finally
		{
			excelFile.delete();
		}
	}
	
	protected void tearDown() throws Exception
	{
		exportFile.delete();
	}
}