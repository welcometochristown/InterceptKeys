# InterceptKeys
Small library used to intercept global key presses

#Usage

        public Form1()
        {
            InitializeComponent();

            InterceptKeys.InterceptedKeyEvent += InterceptKeys_InterceptedKeyEvent;
        }

        private void InterceptKeys_InterceptedKeyEvent(Keys keyCode)
        {
            foreach (var v in InterceptProcessKeys.Where(n => n.Key.Value == keyCode))
            {
                Process p = Process.GetProcessesByName(v.ProcessName).FirstOrDefault();
                if (p != null)
                {
                    //get window handle, make forground and this window will consume the key press
                    IntPtr h = p.MainWindowHandle;
                    SetForegroundWindow(h);
                }
            }
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            InterceptKeys.Hook();
        }

        private void Form1_FormClosing(object sender, FormClosingEventArgs e)
        {
            InterceptKeys.UnHook();
        }
