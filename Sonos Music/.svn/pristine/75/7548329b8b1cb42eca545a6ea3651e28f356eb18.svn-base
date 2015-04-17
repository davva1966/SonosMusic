package sonos;

import java.awt.BorderLayout;
import java.awt.Dimension;
import java.awt.GridBagConstraints;
import java.awt.GridBagLayout;
import java.awt.Insets;
import java.awt.Toolkit;
import java.awt.Dialog.ModalityType;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.util.Properties;
import java.util.concurrent.TimeUnit;

import javax.swing.JButton;
import javax.swing.JDialog;
import javax.swing.JFileChooser;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JProgressBar;
import javax.swing.JTextField;
import javax.swing.SwingWorker;
import javax.swing.WindowConstants;

public class CreateSonosMusicFolderUI implements ActionListener {

	public static String PROPERTIES_FILE = "SonosMusicProperties.xml";
	public static String FROM_DIR = "From directory";
	public static String TO_DIR = "To directory";

	protected JFrame mainFrame = null;

	protected JButton fromDirButton = null;

	protected JTextField toDirText = null;

	protected JButton toDirButton = null;

	protected JTextField fromDirText = null;

	protected JButton runButton = null;

	protected JButton cancelButton = null;

	protected JButton stopButton = null;

	protected JLabel doneLabel = null;

	protected JLabel progressInfoLabel = null;

	protected JProgressBar progressBar = null;

	protected CreateSonosMusicFolder executor = null;

	Properties props = null;

	final static CreateSonosMusicFolderUI ui = new CreateSonosMusicFolderUI();

	private Task task;

	protected JDialog dialog = null;

	protected long startTime = 0;

	protected boolean cancelled = false;

	class Task extends SwingWorker<Void, Void> {

		/*
		 * Main task. Executed in background thread.
		 */
		@Override
		public Void doInBackground() {
			try {
				Thread.sleep(500);
			} catch (InterruptedException e) {
			}
			executor = new CreateSonosMusicFolder();
			executor.addProgressListener(ui);
			executor.run(fromDirText.getText(), toDirText.getText());
			return null;
		}

		/*
		 * Executed in event dispatching thread
		 */
		@Override
		public void done() {
		}
	}

	public CreateSonosMusicFolderUI() {
		super();
	}

	/**
	 * Create the GUI and show it. For thread safety, this method should be
	 * invoked from the event-dispatching thread.
	 */
	protected void createAndShowGUI() {

		// Make sure we have nice window decorations.
		JFrame.setDefaultLookAndFeelDecorated(true);

		// Create and set up the window.
		mainFrame = new JFrame("Create Sonos Music Folder");
		mainFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		// mainFrame.setAlwaysOnTop(true);
		mainFrame.setPreferredSize(new Dimension(700, 160));

		JPanel frame = new JPanel();
		mainFrame.getContentPane().setLayout(new BorderLayout());
		mainFrame.getContentPane().add(frame, BorderLayout.NORTH);

		mainFrame.addWindowListener(new WindowListener() {

			@Override
			public void windowOpened(WindowEvent e) {

			}

			@Override
			public void windowIconified(WindowEvent e) {

			}

			@Override
			public void windowDeiconified(WindowEvent e) {

			}

			@Override
			public void windowDeactivated(WindowEvent e) {

			}

			@Override
			public void windowClosing(WindowEvent e) {

			}

			@Override
			public void windowClosed(WindowEvent e) {
				if (executor != null) {
					executor.stop();
				}

			}

			@Override
			public void windowActivated(WindowEvent e) {

			}
		});

		frame.setLayout(new GridBagLayout());

		// Add dummy row
		JLabel dummy = new JLabel("");
		GridBagConstraints c = new GridBagConstraints();
		c.gridwidth = 3;
		c.ipady = 8;
		c.fill = GridBagConstraints.HORIZONTAL;
		c.weightx = 1.00;
		frame.add(dummy, c);

		// Add from directory
		JLabel fromDirLabel = new JLabel("From directory: ");
		c = new GridBagConstraints();
		c.insets = new Insets(0, 5, 0, 5);
		c.gridy = 1;
		c.ipady = 8;
		frame.add(fromDirLabel, c);

		fromDirText = new JTextField();
		c = new GridBagConstraints();
		c.gridy = 1;
		c.ipady = 8;
		c.fill = GridBagConstraints.HORIZONTAL;
		c.weightx = 1.00;
		frame.add(fromDirText, c);
		fromDirText.setText(getProperties().getProperty(FROM_DIR));

		fromDirButton = new JButton("...");
		fromDirButton.addActionListener(this);
		c = new GridBagConstraints();
		c.gridy = 1;
		c.insets = new Insets(0, 10, 0, 10);
		frame.add(fromDirButton, c);

		// Add dummy row
		dummy = new JLabel("");
		c = new GridBagConstraints();
		c.gridy = 2;
		c.gridwidth = 3;
		c.ipady = 8;
		c.fill = GridBagConstraints.HORIZONTAL;
		c.weightx = 1.00;
		frame.add(dummy, c);

		// Add to directory
		JLabel toDirLabel = new JLabel("To directory: ");
		c = new GridBagConstraints();
		c.insets = new Insets(0, 5, 0, 5);
		c.fill = GridBagConstraints.HORIZONTAL;
		c.gridy = 3;
		c.ipady = 8;
		frame.add(toDirLabel, c);

		toDirText = new JTextField();
		c = new GridBagConstraints();
		c.gridy = 3;
		c.ipady = 8;
		c.fill = GridBagConstraints.HORIZONTAL;
		c.weightx = 1.00;
		frame.add(toDirText, c);
		toDirText.setText(getProperties().getProperty(TO_DIR));

		toDirButton = new JButton("...");
		toDirButton.addActionListener(this);
		c = new GridBagConstraints();
		c.gridy = 3;
		c.insets = new Insets(0, 10, 0, 10);
		frame.add(toDirButton, c);

		// Add dummy row
		dummy = new JLabel("");
		c = new GridBagConstraints();
		c.insets = new Insets(0, 0, 10, 0);
		c.gridy = 7;
		c.gridwidth = 3;
		c.ipady = 8;
		c.fill = GridBagConstraints.HORIZONTAL;
		c.weightx = 1.00;
		frame.add(dummy, c);

		JPanel buttonPanel = new JPanel();
		buttonPanel.setLayout(new GridBagLayout());
		c = new GridBagConstraints();
		c.gridy = 8;
		c.gridx = 0;
		c.gridwidth = 3;
		c.fill = GridBagConstraints.HORIZONTAL;
		c.weightx = 1.00;
		frame.add(buttonPanel, c);

		doneLabel = new JLabel(" ");
		c = new GridBagConstraints();
		c.insets = new Insets(0, 5, 0, 5);
		c.fill = GridBagConstraints.HORIZONTAL;
		c.weightx = 1.00;
		buttonPanel.add(doneLabel, c);

		runButton = new JButton("Run");
		runButton.setPreferredSize(new Dimension(100, runButton.getPreferredSize().height));
		runButton.addActionListener(this);
		c = new GridBagConstraints();
		buttonPanel.add(runButton, c);

		cancelButton = new JButton("Cancel");
		cancelButton.setPreferredSize(new Dimension(100, cancelButton.getPreferredSize().height));
		cancelButton.addActionListener(this);
		c = new GridBagConstraints();
		c.insets = new Insets(0, 5, 0, 5);
		buttonPanel.add(cancelButton, c);

		mainFrame.pack();

		// Determine the new location of the window
		Dimension dim = Toolkit.getDefaultToolkit().getScreenSize();
		int w = mainFrame.getSize().width;
		int h = mainFrame.getSize().height;
		int x = (dim.width - w) / 2;
		int y = (dim.height - h) / 2;

		// Move the window
		mainFrame.setLocation(x, y);

		// Display the window.
		mainFrame.setVisible(true);

	}

	public static void main(String[] args) {

		// Schedule a job for the event-dispatching thread:
		// creating and showing this application's GUI.
		javax.swing.SwingUtilities.invokeLater(new Runnable() {

			public void run() {
				ui.createAndShowGUI();
			}
		});
	}

	@Override
	public void actionPerformed(ActionEvent e) {

		if (e.getSource() == cancelButton) {
			if (executor != null)
				executor.stop();
			mainFrame.dispose();
		}

		if (e.getSource() == stopButton) {
			cancelled = true;
			if (executor != null)
				executor.stop();
		}

		if (e.getSource() == fromDirButton || e.getSource() == toDirButton) {
			JFileChooser chooser = new JFileChooser();
			chooser.setDialogTitle("Select directory");
			chooser.setFileSelectionMode(JFileChooser.DIRECTORIES_ONLY);
			if (e.getSource() == fromDirButton && fromDirText.getText() != null)
				chooser.setSelectedFile(new File(fromDirText.getText().trim()));
			if (e.getSource() == toDirButton && toDirText.getText() != null) 
				chooser.setSelectedFile(new File(toDirText.getText().trim()));

			// disable the "All files" option.
			chooser.setAcceptAllFileFilterUsed(false);

			if (chooser.showOpenDialog(mainFrame) == JFileChooser.APPROVE_OPTION) {
				if (e.getSource() == fromDirButton)
					fromDirText.setText(chooser.getSelectedFile().getAbsolutePath());
				if (e.getSource() == toDirButton)
					toDirText.setText(chooser.getSelectedFile().getAbsolutePath());
			}
		}

		if (e.getSource() == runButton) {
			execute();
		}

	}

	protected void execute() {

		startTime = System.currentTimeMillis();

		saveProperties();

		// Create and set up the dialog.
		dialog = new JDialog(mainFrame, "Creating Sonos Music Folder");
		dialog.setPreferredSize(new Dimension(500, 150));
		dialog.setModalityType(ModalityType.APPLICATION_MODAL);
		dialog.setDefaultCloseOperation(WindowConstants.DO_NOTHING_ON_CLOSE);

		JPanel frame = new JPanel();
		dialog.getContentPane().setLayout(new BorderLayout());
		dialog.getContentPane().add(frame, BorderLayout.NORTH);

		frame.setLayout(new GridBagLayout());

		// Add dummy row
		JLabel dummy = new JLabel("");
		GridBagConstraints c = new GridBagConstraints();
		c.ipady = 8;
		c.fill = GridBagConstraints.HORIZONTAL;
		c.weightx = 1.00;
		frame.add(dummy, c);

		// Add progress bar
		progressInfoLabel = new JLabel(" ");
		c = new GridBagConstraints();
		c.gridy = 1;
		c.gridwidth = 2;
		c.insets = new Insets(0, 10, 0, 10);
		c.fill = GridBagConstraints.HORIZONTAL;
		c.weightx = 1.00;
		frame.add(progressInfoLabel, c);

		progressBar = new JProgressBar();
		c = new GridBagConstraints();
		c.gridy = 2;
		c.gridwidth = 2;
		c.insets = new Insets(10, 10, 0, 10);
		c.fill = GridBagConstraints.HORIZONTAL;
		c.weightx = 1.00;
		frame.add(progressBar, c);

		stopButton = new JButton("Cancel");
		stopButton.addActionListener(this);
		c = new GridBagConstraints();
		c.gridy = 3;
		c.insets = new Insets(10, 0, 10, 10);
		frame.add(stopButton, c);

		dialog.pack();

		// Determine the new location of the window
		Dimension dim = Toolkit.getDefaultToolkit().getScreenSize();
		int w = dialog.getSize().width;
		int h = dialog.getSize().height;
		int x = (dim.width - w) / 2;
		int y = (dim.height - h) / 2;

		// Move the window
		dialog.setLocation(x, y);

		task = new Task();
		task.execute();

		// Display the window.
		dialog.setVisible(true);

	}

	public void startProgress1() {
		progressInfoLabel.setText(" Retreiving files in source directory...");
		progressBar.setIndeterminate(true);

	}

	public void startProgress2(int count) {
		progressInfoLabel.setText("");
		progressBar.setMaximum(count);
		progressBar.setIndeterminate(false);

	}

	public void updateProgress(String info, int number) {
		int numberOfFiles = progressBar.getMaximum();
		progressInfoLabel.setText(" File " + number + " of " + numberOfFiles + ": " + info);
		progressBar.setValue(number);

	}

	public void progressComplete() {
		if (dialog != null)
			dialog.dispose();

		if (cancelled) {
			doneLabel.setText("Sonos folder creation cancelled!");
			cancelled = false;
			return;
		}

		long elapsedTime = System.currentTimeMillis() - startTime;

		StringBuilder sb = new StringBuilder();
		sb.append(" Sonos folder created. ");
		sb.append(progressBar.getMaximum());
		sb.append(" files copied. ");
		sb.append("Elapsed time: ");
		sb.append(getTimeString(elapsedTime));
		doneLabel.setText(sb.toString());

	}

	protected Properties getProperties() {
		if (props == null) {
			props = new Properties();
			try {
				props.loadFromXML(new FileInputStream(PROPERTIES_FILE));
			} catch (Exception e) {
			}
		}

		return props;

	}

	protected void saveProperties() {
		if (props != null) {
			props.setProperty(FROM_DIR, fromDirText.getText());
			props.setProperty(TO_DIR, toDirText.getText());
			try {
				props.storeToXML(new FileOutputStream(PROPERTIES_FILE), null);
			} catch (Exception e) {
			}
		}

	}

	protected String getTimeString(long millis) {
		long hours = TimeUnit.MILLISECONDS.toHours(millis);
		long minutes = TimeUnit.MILLISECONDS.toMinutes(millis);
		long seconds = TimeUnit.MILLISECONDS.toSeconds(millis);

		seconds = seconds - TimeUnit.MINUTES.toSeconds(minutes);
		minutes = minutes - TimeUnit.HOURS.toMinutes(hours);

		StringBuilder sb = new StringBuilder();
		sb.append(hours);
		sb.append(" hrs ");
		sb.append(minutes);
		sb.append(" min ");
		sb.append(seconds);
		sb.append(" sec.");

		return sb.toString();

	}
}
