
"# ACTS-update"


void MainWindowHSS::createParameterBox()
{
	m_pParameterGroupBox = new QGroupBox(this);
	m_pParameterGroupBox->setStyleSheet("QGroupBox {border: 1px solid #555; border-radius: 3px; } ");

	//1. Max Step
	// QLabel *maxStepLabel = new QLabel(tr("Max Step(Exp)"), this);
	QLabel *maxStepLabel = new QLabel(tr("Max Step"), this);
	m_maxStepEdit = new QLineEdit(this);
	connect(m_maxStepEdit, SIGNAL(textChanged(const QString &)), this, SLOT(textEdited(const QString &)));

	//2. Ramp Rate 
	// QLabel *rampRateLabel = new QLabel(tr("Ramp Rate(Exp)"), this);
	QLabel *rampRateLabel = new QLabel(tr("Ramp Rate"), this);
	m_rampRateEdit = new QLineEdit(this);
	connect(m_rampRateEdit, SIGNAL(textChanged(const QString &)), this, SLOT(textEdited(const QString &)));
	m_rampRateEdit->hide(); // Control Ramp Rate at ALC side.
	rampRateLabel->hide();

	//3. Control Tolerance
	// QLabel * controlToleranceLabel = new QLabel(tr("Control Tolerance(Exp)"), this);
	QLabel * controlToleranceLabel = new QLabel(tr("Control Tolerance"), this);
	m_controlToleranceEdit = new QLineEdit(this);
	connect(m_controlToleranceEdit, SIGNAL(textChanged(const QString &)), this, SLOT(textEdited(const QString &)));

	//4. Max Iteration  ---> Come from Analysis Option's  Max Iteration 
	QLabel * maxIterationLabel = new QLabel(tr("Max Iteration"), this);
	m_maxIterationEdit = new QLineEdit(this);
	m_maxIterationEdit->setReadOnly(true);    // set in AnalysisOption
	connect(m_maxIterationEdit, SIGNAL(textChanged(const QString &)), this, SLOT(textEdited(const QString &)));

	//5. Numerical Tolerance ---> Come from Analysis Option's  Error Tolerance 
	QLabel *numericalToleranceLabel = new QLabel(tr("Numerical Tolerance"), this);
	m_numericalToleranceEdit = new QLineEdit(this);
	m_numericalToleranceEdit->setReadOnly(true);	// set in AnalysisOption
	connect(m_numericalToleranceEdit, SIGNAL(textChanged(const QString &)), this, SLOT(textEdited(const QString &)));

	//6. Conversion
	QLabel * conversionLabel = new QLabel(tr("Conversion"), this);
	m_conversionEdit = new QLineEdit(this);
	connect(m_conversionEdit, SIGNAL(textChanged(const QString &)), this, SLOT(textEdited(const QString &)));

	// Set remeber parameter 
	// QSettings settings("ACTS", QCoreApplication::applicationName());
	QSettings settings("ACTS", "DisplacementControlPara");
	DisplacementControlPara para;
	QVariant value = settings.value("DisplacementControlPara", (const QVariant *)&para);
	para = value.value<DisplacementControlPara>();  

	m_maxStepEdit->setText(QString("%1").arg(para.maxStep));
	m_rampRateEdit->setText(QString("%1").arg(para.rampRate));
	m_controlToleranceEdit->setText(QString("%1").arg(para.controlTolerance));          // Keep 6 digits after . 

	m_maxIterationEdit->setText(QString("%1").arg(para.maxIteration));
	m_numericalToleranceEdit->setText(QString("%1").arg(para.numericalTolerance));
	m_conversionEdit->setText(QString("%1").arg(para.conversion));

	QFormLayout *layout = new QFormLayout;
	layout->setVerticalSpacing(10);

	layout->addRow(maxStepLabel, m_maxStepEdit);
	// layout->addRow(rampRateLabel, m_rampRateEdit);
	layout->addRow(controlToleranceLabel, m_controlToleranceEdit);

	layout->addRow(maxIterationLabel, m_maxIterationEdit);
	layout->addRow(numericalToleranceLabel, m_numericalToleranceEdit);
	layout->addRow(conversionLabel, m_conversionEdit);
	layout->addRow(rampRateLabel, m_rampRateEdit);          // Useless in HSS, move to ALC, put at end and hide. 

	m_pParameterGroupBox->setLayout(layout);
}

