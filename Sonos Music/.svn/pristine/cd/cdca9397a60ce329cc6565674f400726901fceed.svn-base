package sonos;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.channels.FileChannel;
import java.util.ArrayList;
import java.util.List;

public class CreateSonosMusicFolder {

	protected boolean stop = false;

	protected CreateSonosMusicFolderUI progressListener = null;

	class FileHelper {

		public FileHelper() {
			super();

		}

		public String getExtension(String fileName) {
			int idx = fileName.lastIndexOf(".");
			if (idx > -1)
				return fileName.substring(idx);

			return null;

		}

		public String getName(String fileName) {
			int idx2 = fileName.lastIndexOf(File.separator) + 1;
			int idx1 = fileName.indexOf(".", idx2);
			if (idx1 > 0) {
				if (idx2 != idx1)
					return fileName.substring(idx2, idx1);
			}

			return null;

		}

		public List<String> listFileNames(String fromDirStr, boolean subdirs) {

			File fromDir = new File(fromDirStr);
			if (fromDir.isDirectory() == false) {
				System.out.println("Invalid from directory");
				return null;
			}
			List<String> fileNameList = new ArrayList<String>();

			return listFileNames(new File(fromDirStr), subdirs, fileNameList);

		}

		public List<String> listFileNames(File fromDir, boolean subdirs, List<String> fileNameList) {

			File[] files = fromDir.listFiles();
			for (File file : files) {
				if (stop)
					break;
				if (file.isDirectory())
					listFileNames(file, subdirs, fileNameList);
				else
					fileNameList.add(file.getAbsolutePath());
			}

			return fileNameList;
		}

		public void copyFile(String fromFileName, String toFileName) throws IOException {
			FileChannel fromChannel = new FileInputStream(fromFileName).getChannel();
			FileChannel toChannel = new FileOutputStream(toFileName).getChannel();
			try {
				// Magic number for Windows, 64Mb - 32Kb)
				int maxCount = (64 * 1024 * 1024) - (32 * 1024);
				long size = fromChannel.size();
				long position = 0;
				while (position < size) {
					position += fromChannel.transferTo(position, maxCount, toChannel);
				}
			} catch (IOException e) {
				throw e;
			} finally {
				if (fromChannel != null)
					fromChannel.close();
				if (toChannel != null)
					toChannel.close();
			}
		}

	}

	public CreateSonosMusicFolder() {
		super();

	}

	public void run(String fromDirStr, String toDirBaseStr) {

		progressListener.startProgress1();

		if (toDirBaseStr.endsWith("\\") == false)
			toDirBaseStr = toDirBaseStr + "\\";

		FileHelper fileHelper = this.new FileHelper();

		List<String> fileNameList = fileHelper.listFileNames(fromDirStr, true);

		progressListener.startProgress2(fileNameList.size());

		int dirCount = 1;
		int count = 1;
		String toDirStr = toDirBaseStr + dirCount + "\\";
		File toDir = new File(toDirStr);
		if (toDir.exists() == false) {
			if (toDir.mkdirs() == false) {
				System.out.println("Unable to create to directory");
				return;
			}
		}

		int fileCount = 0;
		for (String fileName : fileNameList) {
			if (stop)
				break;
			fileCount++;
			progressListener.updateProgress(fileName, fileCount);
			String ext = fileHelper.getExtension(fileName);

			if (fileHelper.getName(fileName) == null)
				continue;
			if (ext == null || ext.trim().toLowerCase().endsWith("m4b"))
				continue;
			if (count >= 300) {
				dirCount++;
				toDirStr = toDirBaseStr + dirCount + "\\";
				toDir = new File(toDirStr);
				toDir.mkdir();
				count = 1;
			}
			try {
				fileHelper.copyFile(fileName, toDirStr + count + ext);
			} catch (Exception e) {
				System.out.println("Error while copying file: " + fileName + ". Error: " + e);
				continue;
			}
			// System.out.println("DirCount: " + dirCount + " FileCount: " +
			// count + " File: " + fileName + " copied");
			count++;
		}

		progressListener.progressComplete();

	}

	public void addProgressListener(CreateSonosMusicFolderUI listener) {
		progressListener = listener;

	}

	public void stop() {
		stop = true;

	}

}
