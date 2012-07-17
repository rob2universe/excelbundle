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

import java.util.ArrayList;
import java.util.Collection;

import senselogic.excelbundle.LanguageFile;
import junit.framework.TestCase;

/**
 * TestCase for LanguageFile.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class LanguageFileTest extends TestCase
{
	public void testSetAndGetValue()
	{
		String value = "foobar";
		LanguageFile langFile = new LanguageFile("foobar", "en");
		langFile.setValue("test", value);
		assertEquals("Wrong value", value, langFile.getValue("test"));
	}
	
	public void testNoEmptyKeyValuePairs()
	{
		LanguageFile langFile = new LanguageFile("foobar", "en");
		assertTrue(
				"Does not return null when no key exists",
				langFile.getValue("foobar") == null);
		
		langFile.setValue("foobar", "whatever");
		langFile.setValue("foobar", null);
		assertTrue(
				"Setting null value creates kvp",
				langFile.getPair("foobar") == null);
		langFile.setValue("foobar", "whatever");
		langFile.setValue("foobar", "   ");
		assertTrue("Setting whitespace string creates kvp",
				langFile.getPair("foobar") == null);
	}
	
	public void testHasSameKeys()
	{
		LanguageFile langFile1 = new LanguageFile("/foobar", "en");
		LanguageFile langFile2 = new LanguageFile("/foobar", "en");
		
		langFile1.setValue("foo1", "foo1");
		langFile1.setValue("foo2", "foo2");
		langFile1.setValue("foo3", "foo3");
		
		langFile2.setValue("foo1", "foo1");
		langFile2.setValue("foo2", "foo2");
		langFile2.setValue("foo3", "foo3");
		
		assertTrue(
				"Reports equal keys as different keys",
				langFile1.hasSameKeys(langFile2));
		
		langFile1 = new LanguageFile("/foobar", "en");
		langFile2 = new LanguageFile("/foobar", "en");
		
		langFile1.setValue("foo1", "foo1");
		langFile1.setValue("foo2", "foo2");
		langFile1.setValue("foo3", "foo3");
		
		langFile2.setValue("foo1", "foo1");
		langFile2.setValue("foo4", "foo2");
		langFile2.setValue("foo3", "foo3");
		
		assertFalse(
				"Reports different keys as equal keys",
				langFile1.hasSameKeys(langFile2));
	}
	
	public void testDiffers()
	{
		LanguageFile langFile1 = new LanguageFile("/foobar", "en");
		LanguageFile langFile2 = new LanguageFile("/foobar", "en");
		
		langFile1.setValue("foo1", "foo1");
		langFile1.setValue("foo2", "foo2");
		langFile1.setValue("foo3", "foo3");
		
		langFile2.setValue("foo1", "foo1");
		langFile2.setValue("foo2", "foo2");
		langFile2.setValue("foo3", "foo3");
		
		assertFalse(
				"Reports equal files different",
				langFile1.differs(langFile2));
		
		langFile1 = new LanguageFile("/foobar", "en");
		langFile2 = new LanguageFile("/foobar", "en");
		
		langFile1.setValue("foo1", "foo1");
		langFile1.setValue("foo2", "foo2");
		langFile1.setValue("foo3", "foo3");
		
		langFile2.setValue("foo1", "foo1");
		langFile2.setValue("foo2", "foo2");
		langFile2.setValue("foo3", "foo31");
		
		assertTrue(
				"Reports equal files different",
				langFile1.differs(langFile2));
		
		langFile1 = new LanguageFile("/foobar", "en");
		langFile2 = new LanguageFile("/foobar", "en");
		
		langFile1.setValue("foo1", "foo1");
		langFile1.setValue("foo2", "foo2");
		langFile1.setValue("foo3", "foo3");
		
		langFile2.setValue("foo1", "foo1");
		langFile2.setValue("foo2", "foo2");
		
		assertTrue(
				"Reports equal files different",
				langFile1.differs(langFile2));
	}
	
	public void testMerge()
	{
		LanguageFile langFile1 = new LanguageFile("/foobar", "en");
		langFile1.setValue("foo1", "foo1");
		langFile1.setValue("foo2", "foo2");
		langFile1.setValue("foo3", "foo3");
		
		LanguageFile langFile2 = new LanguageFile("/foobar", "en");
		langFile2.setValue("foo1", "bar1");
		langFile2.setValue("foo4", "foo4");
		
		LanguageFile expect = new LanguageFile("/foobar", "en");
		expect.setValue("foo1", "bar1");
		expect.setValue("foo2", "foo2");
		expect.setValue("foo3", "foo3");
		
		assertFalse(
				"Unexpected merge result",
				langFile1.merge(langFile2, false).differs(expect));
		
		expect = new LanguageFile("/foobar", "en");
		expect.setValue("foo1", "bar1");
		expect.setValue("foo2", "foo2");
		expect.setValue("foo3", "foo3");
		expect.setValue("foo4", "foo4");
		
		assertFalse(
				"Unexpected merge result",
				langFile1.merge(langFile2, true).differs(expect));
	}
	
	public void testMissingKeySubset()
	{
		LanguageFile ref = new LanguageFile("/foobar", "en");
		ref.setValue("foo1", "bar1");
		ref.setValue("foo2", "bar2");
		ref.setValue("foo3", "bar3");
		ref.setValue("foo4", "bar4");
		
		LanguageFile langFile1 = new LanguageFile("/foobar", "sv");
		langFile1.setValue("foo1", "bar1");
		langFile1.setValue("foo3", "bar3");
		
		LanguageFile langFile2 = new LanguageFile("/foobar", "sv");
		langFile2.setValue("foo1", "bar1");
		langFile2.setValue("foo2", "bar2");
		
		LanguageFile expect = new LanguageFile("/foobar", "en");
		expect.setValue("foo2", "bar2");
		expect.setValue("foo3", "bar3");
		expect.setValue("foo4", "bar4");
		
		Collection<LanguageFile> langs = new ArrayList<LanguageFile>();
		langs.add(langFile1);
		langs.add(langFile2);
		
		assertFalse(expect.differs(ref.missingKeySubset(langs)));
	}
	
	public void testCommonKeySubset()
	{
		LanguageFile langFile1 = new LanguageFile("/foobar1", "en");
		langFile1.setValue("foo1", "bar1");
		langFile1.setValue("foo2", "bar2");
		langFile1.setValue("foo3", "bar3");
		
		LanguageFile langFile2 = new LanguageFile("/foobar1", "en");
		langFile2.setValue("foo2", "foo1");
		langFile2.setValue("foo3", "foo2");
		langFile2.setValue("foo4", "foo3");
		
		LanguageFile expect = new LanguageFile("/foobar1", "en");
		expect.setValue("foo2", "bar2");
		expect.setValue("foo3", "bar3");
		
		assertFalse(langFile1.commonKeySubset(langFile2).differs(expect));
	}
	
	public void testGetFilename()
	{
		assertEquals(
				"/foobar1/test_en.properties",
				LanguageFile.getFilename("/foobar1/test", "en"));
		assertEquals(
				"/foobar1/test.properties",
				LanguageFile.getFilename("/foobar1/test",
						LanguageFile.DEFAULT_LANGUAGE));
	}
}