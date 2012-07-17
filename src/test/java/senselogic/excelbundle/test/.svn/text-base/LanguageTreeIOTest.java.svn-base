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
import java.util.Collection;
import java.util.Properties;

import junit.framework.TestCase;


import senselogic.excelbundle.BundleInfo;
import senselogic.excelbundle.LanguageFile;
import senselogic.excelbundle.LanguagePack;
import senselogic.excelbundle.LanguageTreeIO;

/**
 * TestCase for LanguageTreeIO.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class LanguageTreeIOTest extends TestCase
{
	private LanguageTreeIO langTree;
	private File testFile;
	
	public void setUp() throws IOException
	{
		langTree = 
			new LanguageTreeIO(new File("src/test/").getCanonicalFile());
		testFile = new File(langTree.getRoot(), "writetest_no.properties");
	}
	
	public void testRightBundles()
	{
		Collection<BundleInfo> bundles = langTree.getBundles();
		assertTrue("Wrong number of bundles", bundles.size() == 1);
		
		BundleInfo info = bundles.iterator().next();
		assertEquals(
				"Path is not /senselogic/excelbundle/test/test",
				"/senselogic/excelbundle/test/test",
				info.getPath());
		
		Collection<String> languages = info.getLanguages();
		assertTrue("Wrong number of languages", languages.size() == 3);
		assertTrue("Swedish not found", languages.contains("sv"));
		assertTrue("English not found", languages.contains("en"));
		assertTrue(
				"Default not found",
				languages.contains(LanguageFile.DEFAULT_LANGUAGE));
	}
	
	public void testAvailableLanguages()
	{
		Collection<String> langs = langTree.getAvailableLanguages();
		assertTrue("Wrong number of languages", langs.size() == 3);
		assertTrue("Swedish not found", langs.contains("sv"));
		assertTrue("English not found", langs.contains("en"));
		assertTrue(
				"Default not found",
				langs.contains(LanguageFile.DEFAULT_LANGUAGE));
	}
	
	public void testLoadLanguage() throws IOException
	{
		LanguageFile file = langTree.loadLanguageFile(
				"/senselogic/excelbundle/test/test", "en");
		assertEquals("Incorrect language", "en", file.getLanguage());
		assertEquals(
				"Incorrect path", "/senselogic/excelbundle/test/test",
				file.getPath());
		assertTrue("Wrong number of keys", file.getPairs().size() == 3);
		assertTrue("No such key: test1", file.getPair("test1") != null);
		assertTrue("No such key: test2", file.getPair("test2") != null);
		assertTrue("No such key: test3", file.getPair("test3") != null);
		assertEquals("Wrong value for test1", "Foo en", file.getValue("test1"));
		assertEquals("Wrong value for test2", "Bar en", file.getValue("test2"));
		assertEquals(
				"Wrong value for test3",
				"Foo {0} Bar en", file.getValue("test3"));
	}
	
	public void testSave() throws IOException
	{
		try
		{
			LanguageFile langFile = new LanguageFile("/writetest", "no");
			langFile.setValue("test1", "foo1");
			langFile.setValue("test2", "bar2");
			
			langTree.save(langFile);
			assertTrue(testFile.exists());
			
			Properties prop = new Properties();
			prop.load(new FileInputStream(testFile));
			assertEquals(
					"Wrong value for test1",
					"foo1", prop.getProperty("test1"));
			assertEquals(
					"Wrong value for test2",
					"bar2", prop.getProperty("test2"));
		}
		finally
		{
			testFile.delete();
		}
	}
	
	public void testCache() throws IOException
	{
		LanguagePack pack = langTree.loadLanguage("en");
		LanguagePack pack2 = langTree.loadLanguage("en");
		assertTrue(pack == pack2);
		
		LanguageFile file = langTree.loadLanguageFile(
				"/senselogic/excelbundle/test/test", "en");
		LanguageFile file2 = langTree.loadLanguageFile(
				"/senselogic/excelbundle/test/test", "en");
		assertTrue(file == file2);
	}
}