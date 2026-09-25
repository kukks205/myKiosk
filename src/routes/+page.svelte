<script>
  const patient = {
    cid: '1234567890123',
    hn: '00458291',
    name: 'สมชาย ใจดี',
    age: '45 ปี',
    gender: 'ชาย',
    phone: '081-234-5678',
    right: 'สิทธิหลักประกันสุขภาพแห่งชาติ'
  };

  const departments = [
    {
      id: 'med',
      name: 'อายุรกรรม',
      location: 'อาคารผู้ป่วยนอก ชั้น 2',
      queue: 'A031',
      wait: 'ประมาณ 18 นาที',
      color: '#0f766e'
    },
    {
      id: 'surgery',
      name: 'ศัลยกรรม',
      location: 'อาคารผู้ป่วยนอก ชั้น 3',
      queue: 'S014',
      wait: 'ประมาณ 25 นาที',
      color: '#2563eb'
    },
    {
      id: 'ortho',
      name: 'กระดูกและข้อ',
      location: 'อาคารผู้ป่วยนอก ชั้น 2',
      queue: 'O009',
      wait: 'ประมาณ 32 นาที',
      color: '#c2410c'
    },
    {
      id: 'eye',
      name: 'จักษุ',
      location: 'อาคารผู้ป่วยนอก ชั้น 4',
      queue: 'E022',
      wait: 'ประมาณ 12 นาที',
      color: '#0891b2'
    }
  ];

  let currentStep = 1;
  let inputMode = 'card';
  let selectedDepartment = departments[0];
  let submittedAt = '';

  function startRegistration(mode) {
    inputMode = mode;
    currentStep = 2;
  }

  function confirmVisit() {
    submittedAt = new Intl.DateTimeFormat('th-TH', {
      dateStyle: 'medium',
      timeStyle: 'short'
    }).format(new Date());
    currentStep = 3;
  }

  function resetFlow() {
    currentStep = 1;
    inputMode = 'card';
    selectedDepartment = departments[0];
    submittedAt = '';
  }
</script>

<svelte:head>
  <title>myKiosk | ระบบส่งตรวจอัตโนมัติ</title>
  <meta
    name="description"
    content="Mockup ระบบส่งตรวจอัตโนมัติด้วยตนเองสำหรับโรงพยาบาล"
  />
</svelte:head>

<main class="kiosk-shell">
  <section class="topbar" aria-label="หัวหน้าจอ">
    <div>
      <p class="eyebrow">myKiosk Hospital Self Check-in</p>
      <h1>ระบบลงทะเบียนรับบริการ</h1>
    </div>
    <div class="service-badge">
      <span>พร้อมให้บริการ</span>
      <strong>Self Check-in</strong>
    </div>
  </section>

  {#if currentStep === 1}
    <section class="welcome-screen">
      <div class="instruction-panel">
        <div class="reader-illustration" aria-hidden="true">
          <div class="reader-slot"></div>
          <div class="scan-line"></div>
        </div>
        <p class="eyebrow">เริ่มต้นขั้นตอน</p>
        <h2>กรุณาเสียบบัตรประชาชน หรือแสกนใบนัดเพื่อเริ่มต้นขั้นตอนการลงทะเบียนรับบริการ</h2>
        <p class="instruction-copy">
          ระบบจะอ่าน CID จากบัตรประชาชนเพื่อค้นหา HN หรืออ่าน HN จากใบนัด แล้วไปยังหน้าตรวจสอบข้อมูลผู้ป่วย
        </p>
        <div class="start-actions">
          <button class="primary-action" type="button" on:click={() => startRegistration('card')}>
            เสียบบัตรประชาชน
          </button>
          <button class="secondary-action" type="button" on:click={() => startRegistration('appointment')}>
            แสกนใบนัด
          </button>
        </div>
      </div>
    </section>
  {:else if currentStep === 2}
    <section class="selection-screen">
      <article class="patient-card">
        <div class="patient-header">
          <div class="avatar" aria-hidden="true">สจ</div>
          <div>
            <p class="eyebrow">
              {inputMode === 'card' ? 'อ่านข้อมูลจากบัตรประชาชนสำเร็จ' : 'อ่านข้อมูลจากใบนัดสำเร็จ'}
            </p>
            <h2>{patient.name}</h2>
            <p>HN {patient.hn} · CID {patient.cid}</p>
          </div>
        </div>

        <div class="patient-info">
          <div>
            <span>เพศ / อายุ</span>
            <strong>{patient.gender} · {patient.age}</strong>
          </div>
          <div>
            <span>เบอร์โทร</span>
            <strong>{patient.phone}</strong>
          </div>
          <div class="wide">
            <span>สิทธิการรักษา</span>
            <strong>{patient.right}</strong>
          </div>
        </div>
      </article>

      <article class="department-panel">
        <div class="section-heading">
          <div>
            <p class="eyebrow">เลือกแผนก</p>
            <h2>กรุณาเลือกแผนกที่ต้องการรับบริการ</h2>
          </div>
          <button class="ghost-action" type="button" on:click={resetFlow}>เริ่มใหม่</button>
        </div>

        <div class="department-grid">
          {#each departments as department}
            <button
              class:selected={selectedDepartment.id === department.id}
              class="department-card"
              style={`--accent: ${department.color}`}
              type="button"
              on:click={() => (selectedDepartment = department)}
            >
              <span class="department-mark"></span>
              <strong>{department.name}</strong>
              <small>{department.location}</small>
              <em>คิวล่าสุด {department.queue} · รอ {department.wait}</em>
            </button>
          {/each}
        </div>

        <div class="confirm-bar">
          <div>
            <span>แผนกที่เลือก</span>
            <strong>{selectedDepartment.name}</strong>
          </div>
          <button class="primary-action compact" type="button" on:click={confirmVisit}>
            ยืนยันส่งตรวจ
          </button>
        </div>
      </article>
    </section>
  {:else}
    <section class="result-screen">
      <article class="result-card">
        <div class="success-symbol" aria-hidden="true">✓</div>
        <p class="eyebrow">ลงทะเบียนรับบริการสำเร็จ</p>
        <h2>รายละเอียดการส่งตรวจ</h2>

        <div class="queue-number">
          <span>คิว</span>
          <strong>{selectedDepartment.queue}</strong>
        </div>

        <div class="result-details">
          <div>
            <span>แผนก</span>
            <strong>{selectedDepartment.name}</strong>
          </div>
          <div>
            <span>HN</span>
            <strong>{patient.hn}</strong>
          </div>
          <div>
            <span>ชื่อ-สกุล</span>
            <strong>{patient.name}</strong>
          </div>
          <div>
            <span>เวลาที่ส่งตรวจ</span>
            <strong>{submittedAt}</strong>
          </div>
        </div>

        <p class="result-note">กรุณารอเรียกคิวที่ {selectedDepartment.location}</p>
        <button class="secondary-action" type="button" on:click={resetFlow}>กลับหน้าแรก</button>
      </article>
    </section>
  {/if}

  <section class="stepper bottom-stepper" aria-label="ขั้นตอนการลงทะเบียน">
    <div class:active={currentStep === 1} class:done={currentStep > 1} class="step">
      <span>1</span>
      เริ่มต้น
    </div>
    <div class:active={currentStep === 2} class:done={currentStep > 2} class="step">
      <span>2</span>
      เลือกแผนก
    </div>
    <div class:active={currentStep === 3} class="step">
      <span>3</span>
      รายละเอียดส่งตรวจ
    </div>
  </section>
</main>

<style>
  .kiosk-shell {
    min-height: 100vh;
    padding: clamp(20px, 4vw, 48px);
    padding-bottom: clamp(96px, 11vw, 128px);
    background:
      radial-gradient(circle at top left, rgba(14, 165, 164, 0.16), transparent 32rem),
      linear-gradient(135deg, #f6fafc 0%, #eef7f4 46%, #f8f7f0 100%);
    color: #14213d;
  }

  .topbar,
  .stepper,
  .welcome-screen,
  .selection-screen,
  .result-screen {
    width: min(1240px, 100%);
    margin: 0 auto;
  }

  .topbar {
    display: grid;
    align-items: center;
    justify-items: center;
    gap: 16px;
    margin-bottom: 22px;
    text-align: center;
  }

  .eyebrow {
    margin: 0 0 8px;
    color: #5d7083;
    font-size: 0.82rem;
    font-weight: 800;
    letter-spacing: 0;
    text-transform: uppercase;
  }

  h1,
  h2,
  p {
    margin-top: 0;
  }

  h1 {
    margin-bottom: 0;
    color: #10243f;
    font-size: clamp(2rem, 4vw, 3.45rem);
    line-height: 1.05;
  }

  h2 {
    margin-bottom: 0;
    color: #10243f;
    font-size: clamp(1.45rem, 2.3vw, 2.15rem);
    line-height: 1.25;
  }

  .service-badge {
    display: grid;
    min-width: 180px;
    padding: 16px 18px;
    border: 1px solid rgba(27, 52, 82, 0.08);
    border-radius: 8px;
    background: rgba(255, 255, 255, 0.78);
    box-shadow: 0 18px 50px rgba(36, 54, 74, 0.08);
    text-align: center;
  }

  .service-badge span {
    color: #64748b;
    font-weight: 800;
  }

  .service-badge strong {
    color: #0f766e;
    font-size: 1.35rem;
  }

  .stepper {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
  }

  .bottom-stepper {
    position: fixed;
    right: clamp(16px, 4vw, 48px);
    bottom: clamp(16px, 3vw, 28px);
    left: clamp(16px, 4vw, 48px);
    z-index: 10;
    width: auto;
    max-width: 1240px;
    margin: 0 auto;
    padding: 10px;
    border: 1px solid rgba(27, 52, 82, 0.08);
    border-radius: 8px;
    background: rgba(248, 251, 252, 0.9);
    box-shadow: 0 18px 56px rgba(42, 60, 82, 0.14);
    backdrop-filter: blur(14px);
  }

  .step {
    display: flex;
    align-items: center;
    gap: 10px;
    min-height: 58px;
    padding: 12px 14px;
    border: 1px solid rgba(30, 41, 59, 0.08);
    border-radius: 8px;
    background: rgba(255, 255, 255, 0.62);
    color: #64748b;
    font-weight: 900;
  }

  .step span {
    display: grid;
    width: 32px;
    height: 32px;
    place-items: center;
    border-radius: 999px;
    background: #dce8ee;
    color: #355066;
  }

  .step.active,
  .step.done {
    border-color: rgba(15, 118, 110, 0.28);
    background: #ffffff;
    color: #0f766e;
  }

  .step.active span,
  .step.done span {
    background: #0f766e;
    color: #ffffff;
  }

  .welcome-screen,
  .result-screen {
    display: grid;
    min-height: min(620px, calc(100vh - 260px));
    place-items: center;
  }

  .instruction-panel,
  .result-card,
  .patient-card,
  .department-panel {
    border: 1px solid rgba(27, 52, 82, 0.08);
    border-radius: 8px;
    background: rgba(255, 255, 255, 0.92);
    box-shadow: 0 18px 56px rgba(42, 60, 82, 0.1);
  }

  .instruction-panel {
    display: grid;
    justify-items: center;
    width: min(860px, 100%);
    padding: clamp(28px, 5vw, 56px);
    text-align: center;
  }

  .reader-illustration {
    position: relative;
    display: grid;
    width: min(320px, 80vw);
    height: 190px;
    margin-bottom: 30px;
    place-items: center;
    border-radius: 8px;
    background: linear-gradient(180deg, #23384e, #10243f);
  }

  .reader-slot {
    width: 180px;
    height: 22px;
    border-radius: 999px;
    background: #c8d6e2;
    box-shadow: 0 46px 0 rgba(20, 184, 166, 0.92);
  }

  .scan-line {
    position: absolute;
    right: 42px;
    bottom: 38px;
    width: 90px;
    height: 90px;
    border: 3px solid #d8e6ef;
    border-radius: 8px;
    box-shadow: inset 0 -28px 0 rgba(20, 184, 166, 0.22);
  }

  .instruction-panel h2 {
    max-width: 780px;
    font-size: clamp(2rem, 4vw, 3.2rem);
  }

  .instruction-copy {
    max-width: 720px;
    margin: 18px 0 28px;
    color: #52677c;
    font-size: clamp(1rem, 1.6vw, 1.22rem);
    line-height: 1.65;
  }

  .start-actions {
    display: grid;
    grid-template-columns: repeat(2, minmax(220px, 1fr));
    gap: 14px;
    width: min(560px, 100%);
  }

  button {
    border-radius: 8px;
    font-weight: 900;
  }

  .primary-action,
  .secondary-action,
  .ghost-action {
    min-height: 58px;
    padding: 0 22px;
  }

  .primary-action {
    border: 0;
    background: #0f766e;
    color: #ffffff;
    box-shadow: 0 14px 24px rgba(15, 118, 110, 0.22);
  }

  .primary-action:hover {
    background: #0b5f59;
  }

  .secondary-action {
    border: 1px solid #b8cbd5;
    background: #ffffff;
    color: #18334a;
  }

  .ghost-action {
    border: 1px solid #d7e1e8;
    background: #f8fbfc;
    color: #41566b;
  }

  .compact {
    min-width: 190px;
  }

  .selection-screen {
    display: grid;
    grid-template-columns: 0.86fr 1.35fr;
    gap: 18px;
    align-items: start;
  }

  .patient-card,
  .department-panel,
  .result-card {
    padding: clamp(20px, 2.8vw, 30px);
  }

  .patient-header {
    display: flex;
    gap: 16px;
    align-items: center;
    margin-bottom: 20px;
  }

  .patient-header p:last-child {
    margin-bottom: 0;
    color: #5f7182;
    font-weight: 800;
  }

  .avatar {
    display: grid;
    width: 74px;
    height: 74px;
    flex: 0 0 74px;
    place-items: center;
    border-radius: 50%;
    background: linear-gradient(135deg, #0f766e, #339af0);
    color: #ffffff;
    font-size: 1.45rem;
    font-weight: 900;
  }

  .patient-info,
  .result-details {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 12px;
  }

  .patient-info div,
  .result-details div {
    display: grid;
    gap: 5px;
    min-height: 82px;
    padding: 14px;
    border: 1px solid #dce5eb;
    border-radius: 8px;
    background: #fbfdfe;
  }

  .patient-info .wide {
    grid-column: 1 / -1;
  }

  .patient-info span,
  .result-details span,
  .confirm-bar span,
  .queue-number span {
    color: #64748b;
    font-size: 0.9rem;
    font-weight: 800;
  }

  .patient-info strong,
  .result-details strong,
  .confirm-bar strong {
    color: #1f3349;
    line-height: 1.35;
  }

  .section-heading {
    display: flex;
    align-items: end;
    justify-content: space-between;
    gap: 16px;
    margin-bottom: 16px;
  }

  .department-grid {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 12px;
  }

  .department-card {
    display: grid;
    grid-template-columns: 12px 1fr;
    gap: 8px 12px;
    min-height: 126px;
    padding: 16px;
    border: 1px solid #dce5eb;
    background: #ffffff;
    color: #22364d;
    text-align: left;
  }

  .department-card.selected {
    border-color: var(--accent);
    box-shadow: 0 14px 34px rgba(15, 118, 110, 0.14);
  }

  .department-mark {
    grid-row: 1 / 4;
    width: 12px;
    height: 100%;
    border-radius: 999px;
    background: var(--accent);
  }

  .department-card strong {
    font-size: 1.24rem;
  }

  .department-card small,
  .department-card em {
    color: #65788b;
    font-style: normal;
    font-weight: 750;
  }

  .confirm-bar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 16px;
    margin-top: 18px;
    padding: 16px;
    border-radius: 8px;
    background: #eef8f6;
  }

  .confirm-bar div {
    display: grid;
    gap: 4px;
  }

  .result-card {
    display: grid;
    justify-items: center;
    width: min(780px, 100%);
    text-align: center;
  }

  .success-symbol {
    display: grid;
    width: 74px;
    height: 74px;
    margin-bottom: 16px;
    place-items: center;
    border-radius: 50%;
    background: #0f766e;
    color: #ffffff;
    font-size: 2.4rem;
    font-weight: 900;
  }

  .queue-number {
    display: grid;
    gap: 6px;
    width: min(360px, 100%);
    margin: 24px 0;
    padding: 24px;
    border-radius: 8px;
    background: #eef8f6;
    color: #0d665f;
  }

  .queue-number strong {
    color: #0f766e;
    font-size: clamp(3.8rem, 11vw, 6rem);
    line-height: 0.95;
  }

  .result-details {
    width: 100%;
    text-align: left;
  }

  .result-note {
    margin: 22px 0 16px;
    color: #52677c;
    font-size: 1.08rem;
    font-weight: 800;
  }

  @media (max-width: 920px) {
    .selection-screen {
      grid-template-columns: 1fr;
    }
  }

  @media (max-width: 760px) {
    .kiosk-shell {
      padding: 16px;
      padding-bottom: 190px;
    }

    .topbar,
    .section-heading,
    .confirm-bar {
      align-items: stretch;
      flex-direction: column;
    }

    .stepper,
    .start-actions,
    .department-grid,
    .patient-info,
    .result-details {
      grid-template-columns: 1fr;
    }

    .bottom-stepper {
      right: 12px;
      bottom: 12px;
      left: 12px;
      padding: 8px;
    }

    .patient-info .wide {
      grid-column: auto;
    }

    .patient-header {
      align-items: flex-start;
    }

    .compact {
      width: 100%;
    }
  }
</style>
