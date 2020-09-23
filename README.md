<div align="center">

## PORG \- PORG Organizes Real Good


</div>

### Description

This utility lets you take a directory of files, and rename them all sequentially with your choice of prefix. For example, if the directory had 400 images, all with different formats of names, it will change them to something like IMG0000.jpg, IMG0001.jpg, IMG0002.jpg... DON'T FORGET TO RATE MY CODE!
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Josh Sherman](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/josh-sherman.md)
**Level**          |Intermediate
**User Rating**    |4.3 (34 globes from 8 users)
**Compatibility**  |PHP 4\.0
**Category**       |[Files](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/files__8-2.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/josh-sherman-porg-porg-organizes-real-good__8-576/archive/master.zip)





### Source Code

```
<?
/*
 * porg.php - PORG Organizes Real Good
 *
 * Author: Josh Sherman
 * Purpose: Renames a directory of files based
 * on a custom prefix. i.e. PORGn.*
 * Usage: php -q porg.php
 */
if (!class_exists('gtk')) {
	if (strtoupper(substr(PHP_OS, 0, 3)) == 'WIN')
		dl('php_gtk.dll');
	else
		dl('php_gtk.so');
}
function delete_event()
{
	return false;
}
function destroy()
{
	Gtk::main_quit();
}
function back_up()
{
	global $dir_entry;
	global $directory;
	$directory = $dir_entry->get_text();
	@mkdir("$directory/bkup", 0777);
	if ($dir = @opendir("$directory")) {
		while (($file = readdir($dir)) !== false) {
			if ($file != "bkup" && substr($file, 0, 1) != "." && is_dir($file) == 0) {
				if (@copy("$directory/$file", "$directory/bkup/$file")) {
					unlink("$directory/$file");
				}
			}
		}
		closedir($dir);
	}
	rename_files();
}
function rename_files()
{
	global $directory;
	global $prefix;
	global $prefix_entry;
	global $check;
	global $window;
	$prefix = $prefix_entry->get_text();
	$i = 0;
	if ($dir = opendir("$directory/bkup")) {
		while (($file = readdir($dir)) !== false) {
			if (strlen($i) == 1) { $number = "000" . $i; }
			if (strlen($i) == 2) { $number = "00" . $i; }
			if (strlen($i) == 3) { $number = "0" . $i; }
			$extension = substr(strrchr($file, "."), 1);
			if ($file != "." && $file != ".." && $file != "bkup") {
				if (@copy("$directory/bkup/$file", "$directory/$prefix$number.$extension")) {
					if ($check->get_active() == 0) {
						unlink("$directory/bkup/$file");
					}
					$i++;
				}
			}
		}
		closedir($dir);
	}
	if ($check->get_active() == 0) {
		rmdir("$directory/bkup");
	}
	echo "\nall done!\n";
}
$window = &new GtkWindow();
$window->set_title('PORG');
$window->connect('destroy', 'destroy');
$window->connect('delete-event', 'delete_event');
$window->set_border_width(10);
$table = &new GtkTable(4, 2);
$table->set_row_spacings(4);
$table->set_col_spacings(4);
$window->add($table);
$dir_label = &new GtkLabel('Directory: ');
$table->attach($dir_label, 0, 1, 0, 1);
$prefix_label = &new GtkLabel('File Prefix: ');
$table->attach($prefix_label, 0, 1, 1, 2);
$dir_entry = &new GtkEntry();
$table->attach($dir_entry, 1, 2, 0, 1);
$prefix_entry = &new GtkEntry();
$table->attach($prefix_entry, 1, 2, 1, 2);
$check = &new GtkCheckButton('Backup Directory?');
$check->set_active(TRUE);
$table->attach($check, 1, 2, 2, 3);
$button = &new GtkButton('Rename Files');
$button->connect('clicked','back_up');
$table->attach($button, 0, 2, 3, 4);
$window->show_all();
Gtk::main();
?>
```

