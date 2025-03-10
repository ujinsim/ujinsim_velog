<h2 id="✨-띵동-프로젝트--폼지-제작-기능">✨ 띵동 프로젝트 – 폼지 제작 기능</h2>
<blockquote>
<h3 id="띵동이란">띵동이란?</h3>
<p><strong>명지대학교</strong> 동아리 <strong>통합 플랫폼</strong>으로,
학생들이 <strong>파편화된 동아리 정보</strong>와 <strong>비효율적인 동아리 업무</strong>를
<strong>일원화하여 제공하는 서비스</strong>입니다.</p>
</blockquote>
<h3 id="기존-방식과-개선점">기존 방식과 개선점</h3>
<p>기존에는 동아리 지원 시, 해당 동아리가 올린 <strong>외부 폼지(구글 폼 등)</strong>로 이동해야 했어요.</p>
<p>💡 <strong>개선된 방식 → 띵동 자체 폼지</strong>를 제작!
이제 <strong>지원하기 버튼</strong>을 누르면 <strong>띵동 내부 폼지</strong>로 이동합니다.</p>
<hr />
<h3 id="폼지-구성">폼지 구성</h3>
<h4 id="✅-기본-질문-공통">✅ 기본 질문 (공통)</h4>
<p>동아리 지원서 특성상, 모든 지원자가 <strong>공통으로 입력해야 하는 항목</strong>이 있어요.</p>
<ul>
<li>이름</li>
<li>학번</li>
<li>학과</li>
<li>전화번호</li>
<li>이메일</li>
</ul>
<h4 id="✅-맞춤형-질문-시트-추가-기능">✅ 맞춤형 질문 (시트 추가 기능)</h4>
<p>모집 분야가 여러 개인 동아리는 <strong>시트를 추가</strong>할 수 있어요.</p>
<blockquote>
<p><strong>예를 들어,</strong>
밴드부 → 보컬, 기타, 드럼 등 각 파트별 추가 질문 가능
사용자가 보컬 선택 → 공통 질문 + 보컬 질문 응답</p>
</blockquote>
<h4 id="✅-질문-유형-5가지">✅ 질문 유형 (5가지)</h4>
<ul>
<li>파일 업로드</li>
<li>체크박스 (다중 선택 가능)</li>
<li>객관식 (단일 선택)</li>
<li>단답형 (300자 이내)</li>
<li>서술형 (1000자 이내)
각 질문은 옵션 및 필수 여부를 설정할 수 있습니다.</li>
</ul>
<hr />
<h3 id="폼지-관리-기능-admin">폼지 관리 기능 (Admin)</h3>
<p>Admin 페이지에서** 지원서의 주요 정보**를 관리할 수 있어요.</p>
<ul>
<li>지원서 제목</li>
<li>모집 기간</li>
<li>지원서 설명</li>
<li>면접 여부 설정</li>
</ul>
<hr />
<h3 id="개발-난이도--복잡도">개발 난이도 &amp; 복잡도</h3>
<p>처음엔 단순히 폼지만 만든다고 생각했는데…
막상 개발하다 보니 <strong>엄청난 복잡도</strong>가 있었습니다. </p>
<p>-&gt; 하나의 컴포넌트에서 모든 상태 관리
<strong>ManageComponent</strong>에서</p>
<ul>
<li>새 지원서 생성</li>
<li>수정 중인 경우</li>
<li>모집 기간 전</li>
<li>모집 기간 중
모두 <strong>하나의 컴포넌트에서 관리</strong>해야 했어요.</li>
</ul>
<p>초기 코드를 복잡도를 고려하지 않고 작성하면…</p>
<ul>
<li>유지보수 어려움</li>
<li>예외 처리 부족
기능 추가 시 구조적으로 수정해야 하는 부분이 많아져서 <strong>확장성을 고려한 설계</strong>가 정말 중요하다는 걸 느꼈습니다. </li>
</ul>
<p>이를 고려하지 않고 코드를 짠 결과 
제 안타까운 <strong>ManageForm</strong>컴포넌트를 소개해보겠습니다. . . ☠️</p>
<pre><code>import React, { useState, useEffect } from 'react';
import Head from 'next/head';
import Image from 'next/image';
import { useRouter } from 'next/router';
import { useCookies } from 'react-cookie';
import toast from 'react-hot-toast';
import Datepicker from 'react-tailwindcss-datepicker';
import { DateRangeType } from 'react-tailwindcss-datepicker/dist/types';
import arrow_left from '@/assets/arrow_left.svg';
import BaseInput from '@/components/apply/BaseInput';
import CommonQuestion from '@/components/apply/CommnQuestion';
import Question from '@/components/apply/Question';
import Sections from '@/components/apply/Sections';
import TextArea from '@/components/apply/TextArea';
import { useNewForm } from '@/hooks/api/apply/useNewForm';
import { useUpdateForm } from '@/hooks/api/apply/useUpdateForm';
import {
  FormData,
  QuestionType,
  SectionFormField,
  FormField,
} from '@/types/form';
import FormEditButtons from './FormEditButtons';
import AddForm from '../../assets/add_form.svg';
import square from '../../assets/checkbox.svg';
import emptySquare from '../../assets/empty_square_check.svg';
import Heading from '../common/Heading';

interface Props {
  formData?: FormData;
  id?: number;
  onReset?: () =&gt; void;
}
interface CategorizedFields {
  [key: string]: FormField[];
}

export default function ManageForm({ formData, id, onReset }: Props) {
  const router = useRouter();
  const [{ token }] = useCookies(['token']);
  const newFormMutation = useNewForm(token);
  const [isEditing, setIsEditing] = useState(false);
  const updateFormMutation = useUpdateForm(setIsEditing);

  const [title, setTitle] = useState(formData?.title ? formData.title : '');
  const [description, setDescription] = useState(
    formData?.description ? formData.description : '',
  );

  useEffect(() =&gt; {
    if (formData) {
      const updatedFormFields = Object.keys(categorizeFormFields(formData)).map(
        (section) =&gt; ({
          section,
          questions: categorizeFormFields(formData)[section].map((field) =&gt; ({
            question: field.question,
            type: field.type,
            options: field.options || [],
            required: field.required,
            order: field.order,
            section: field.section,
          })),
        }),
      );

      setFormField(updatedFormFields);
    }
  }, [formData]);

  const handleCreateForm = () =&gt; {
    if (!title) {
      toast.error('지원서 제목을 입력하여주세요. ');
    }

    if (!description || description.length &gt; 255) {
      toast.error('지원서 설명은 255자 이내로 작성하여주세요.');
      return;
    }

    const formattedPostData = formatFormData();
    newFormMutation.mutate(formattedPostData);
  };

  const handleUpdateForm = () =&gt; {
    if (!id || !formData) {
      toast.error('수정할 폼이 존재하지 않습니다.');
      return;
    }
    if (!title) {
      toast.error('지원서 제목을 입력하여주세요. ');
    }

    if (!description || description.length &gt; 255) {
      toast.error('지원서 설명은 255자 이내로 작성하여주세요.');
      return;
    }

    const formattedPostData = formatFormData();

    updateFormMutation.mutate({
      token,
      formId: id,
      formData: formattedPostData,
    });
    setIsEditing(false);
    setIsClosed(true);
  };

  const formatFormData = (): FormData =&gt; {
    const formatDate = (date: Date | string | null) =&gt; {
      if (!date) return '';
      if (date instanceof Date) return date.toISOString().split('T')[0];
      return new Date(date).toISOString().split('T')[0];
    };

    return {
      title: title.trim(),
      description: description.trim() || null,
      startDate: formatDate(recruitPeriod.startDate),
      endDate: formatDate(recruitPeriod.endDate),
      hasInterview: isChecked ?? false,
      sections: sections,
      formFields: formField.flatMap((section) =&gt;
        section.questions.map(
          (question): FormField =&gt; ({
            question: question.question.trim(),
            type: question.type as QuestionType,
            options: question.options || [],
            required: question.required,
            order: question.order,
            section: section.section,
          }),
        ),
      ),
    };
  };

  const isPastStartDate = formData?.startDate
    ? new Date(formData.startDate) &lt; new Date()
    : false;

  const [isClosed, setIsClosed] = useState(formData ? true : false);

  const [isChecked, setIsChecked] = useState(formData?.hasInterview);

  function categorizeFormFields(
    formData: FormData | undefined,
  ): CategorizedFields {
    const categorizedFields: CategorizedFields = {};

    (formData?.sections || []).forEach((section) =&gt; {
      categorizedFields[section] = [];
    });

    (formData?.formFields || []).forEach((field) =&gt; {
      if (field.section in categorizedFields) {
        categorizedFields[field.section].push(field);
      }
    });

    return categorizedFields;
  }

  const [modiformField, setmodiFormField] = useState(
    Object.keys(categorizeFormFields(formData)).map((section) =&gt; ({
      section,
      questions: categorizeFormFields(formData)[section].map((field) =&gt; ({
        question: field.question,
        type: field.type,
        options: field.options || ['옵션1'],
        required: field.required,
        order: field.order,
        section,
      })),
    })),
  );

  const [focusSection, setFocusSection] = useState('공통');
  const [sections, setSections] = useState(
    formData ? formData.sections : ['공통'],
  );
  const [recruitPeriod, setRecruitPeriod] = useState&lt;DateRangeType&gt;({
    startDate: null,
    endDate: null,
  });

  useEffect(() =&gt; {
    if (formData) {
      setRecruitPeriod({
        startDate: formData.startDate ? new Date(formData.startDate) : null,
        endDate: formData.endDate ? new Date(formData.endDate) : null,
      });
    }
  }, [formData]);

  const baseQuestion: FormField[] = [
    {
      question: '',
      type: 'RADIO',
      options: ['옵션1'],
      required: true,
      order: 1,
      section: '공통',
    },
  ];

  const [formField, setFormField] = useState&lt;SectionFormField[]&gt;(
    formData
      ? modiformField
      : [
          {
            section: '공통',
            questions: [
              {
                question: '',
                type: 'RADIO',
                options: ['옵션1'],
                required: true,
                order: 1,
                section: '공통',
              },
            ],
          },
        ],
  );

  const handleDateChange = (newValue: DateRangeType | null) =&gt; {
    setRecruitPeriod(newValue ?? { startDate: null, endDate: null });
  };

  const deleteQuestion = (sectionName: string, questionIndex: number) =&gt; {
    setFormField((prev) =&gt;
      prev.map((section) =&gt;
        section.section === sectionName
          ? {
              ...section,
              questions: section.questions
                .filter((_, qIndex) =&gt; qIndex !== questionIndex)
                .map((question, newIndex) =&gt; ({
                  ...question,
                  order: newIndex + 1,
                })),
            }
          : section,
      ),
    );
  };

  const [modalVisible, setModalVisible] = useState(false);
  const [newSectionName, setNewSectionName] = useState('');

  const handleOpenModal = () =&gt; {
    setNewSectionName('');
    setModalVisible(true);
  };

  const onClickEditButton = () =&gt; {
    setIsEditing(true);
    setIsClosed(false);
  };

  const onClickCancelButton = () =&gt; {
    onReset?.();
  };

  const addQuestion = () =&gt; {
    setFormField((prev) =&gt;
      prev.map((section) =&gt;
        section.section === focusSection
          ? {
              ...section,
              questions: [
                ...section.questions,
                {
                  question: '',
                  type: 'RADIO',
                  options: ['옵션1'],
                  required: true,
                  order: section.questions.length + 1,
                  section: focusSection,
                },
              ],
            }
          : section,
      ),
    );
  };

  return (
    &lt;div&gt;
      &lt;Head&gt;
        &lt;title&gt;지원서 템플릿 관리&lt;/title&gt;
      &lt;/Head&gt;

      &lt;div className=&quot;flex items-center justify-between gap-2&quot;&gt;
        &lt;div
          onClick={() =&gt; router.back()}
          className=&quot;flex cursor-pointer items-center gap-1 &quot;
        &gt;
          &lt;Image
            src={arrow_left}
            alt=&quot;navigation&quot;
            className=&quot;mt-7 w-6 md:mt-11 md:w-9&quot;
          /&gt;
          &lt;Heading&gt;지원서 생성&lt;/Heading&gt;
        &lt;/div&gt;
        &lt;FormEditButtons
          formData={formData}
          isEditing={isEditing}
          isClosed={isClosed}
          isPastStartDate={isPastStartDate}
          handleCreateForm={handleCreateForm}
          onClickEditButton={onClickEditButton}
          onClickCancelButton={onClickCancelButton}
          handleUpdateForm={handleUpdateForm}
        /&gt;
      &lt;/div&gt;

      &lt;div className=&quot;flex w-full items-center justify-end gap-2 pt-10 text-lg font-semibold text-gray-500&quot;&gt;
        &lt;div className=&quot;relative flex h-[20px] w-[20px] cursor-pointer items-center justify-center&quot;&gt;
          &lt;Image
            onClick={() =&gt; {
              if (!isClosed) {
                setIsChecked(!isChecked);
              }
            }}
            src={isChecked ? square : emptySquare}
            width={isChecked ? 18 : 22}
            height={isChecked ? 18 : 22}
            className={`object-contain ${
              isClosed ? 'cursor-not-allowed opacity-50' : 'cursor-pointer'
            }`}
            alt=&quot;checkBox&quot;
          /&gt;
        &lt;/div&gt;
        우리동아리는 면접을 보지 않아요!
      &lt;/div&gt;

      &lt;div className=&quot;flex flex-col gap-4&quot;&gt;
        &lt;div className=&quot;mt-4 flex flex-row flex-wrap gap-3 md:flex-nowrap&quot;&gt;
          &lt;BaseInput
            type=&quot;text&quot;
            placeholder={'지원서 제목을 입력해주세요'}
            value={title}
            onChange={(
              e: React.ChangeEvent&lt;HTMLInputElement | HTMLTextAreaElement&gt;,
            ) =&gt; setTitle(e.target.value)}
            disabled={isClosed}
          /&gt;

          &lt;div className=&quot;w-full rounded-lg border pt-1&quot;&gt;
            &lt;Datepicker
              value={recruitPeriod}
              useRange={false}
              minDate={new Date(new Date().getFullYear(), 0, 1)}
              maxDate={new Date(new Date().getFullYear(), 11, 31)}
              onChange={handleDateChange}
              placeholder=&quot;모집 기간을 설정하세요&quot;
              disabled={isClosed}
            /&gt;
          &lt;/div&gt;
        &lt;/div&gt;
        &lt;TextArea
          placeholder=&quot;지원서 설명을 입력해 주세요 (최대 255자 이내)&quot;
          value={description}
          onChange={(
            e: React.ChangeEvent&lt;HTMLTextAreaElement | HTMLInputElement&gt;,
          ) =&gt; setDescription(e.target.value)}
          disabled={isClosed}
        /&gt;
      &lt;/div&gt;

      &lt;div className=&quot;mt-6&quot;&gt;
        &lt;Sections
          addSection={handleOpenModal}
          focusSection={focusSection}
          sections={sections}
          setFocusSection={setFocusSection}
          isClosed={isClosed}
          formField={formField}
          setFormField={setFormField}
          setSections={setSections}
          baseQuestion={baseQuestion}
        /&gt;
        {focusSection == '공통' &amp;&amp; &lt;CommonQuestion disabled={true} /&gt;}

        {formField
          .filter((item) =&gt; item.section === focusSection)
          .map((section) =&gt; (
            &lt;div key={section.section}&gt;
              {section.questions.map((question, qIndex) =&gt; (
                &lt;Question
                  key={`${section.section}-${qIndex}`}
                  index={qIndex}
                  questionData={question}
                  deleteQuestion={() =&gt; deleteQuestion(section.section, qIndex)}
                  setFormField={setFormField}
                  section={section}
                  isClosed={isClosed}
                /&gt;
              ))}
            &lt;/div&gt;
          ))}
      &lt;/div&gt;
      {!isClosed &amp;&amp; (
        &lt;button
          onClick={addQuestion}
          className=&quot;fixed bottom-24 right-[calc(10vw)] flex items-center justify-center rounded-full bg-blue-500 p-1 shadow-lg transition-all duration-200 hover:bg-blue-600 md:right-[calc(5vw)] lg:right-[calc(2vw)]&quot;
        &gt;
          &lt;Image src={AddForm} width={40} height={40} alt=&quot;질문 추가하기&quot; /&gt;
        &lt;/button&gt;
      )}
    &lt;/div&gt;
  );
}</code></pre><hr />
<h3 id="주요-문제점정리">주요 문제점..정리</h3>
<h4 id="1-과도한-상태state-관리">1. 과도한 상태(state) 관리</h4>
<ul>
<li><strong>isEditing, isClosed, isPastStartDate</strong> 등 권한과 관련된 상태들이 마구잡이로 관리됨</li>
<li><strong>formData, sections, formFields</strong> 등 폼지의 필드들도 각각의 state로 관리됨
결과적으로 수정이 어려운 코드가 되어버림</li>
</ul>
<h4 id="2-불필요한-데이터-변환">2. 불필요한 데이터 변환</h4>
<p>formData를 섹션별로 객체화하여 관리했는데, </p>
<ul>
<li>막상 섹션 추가하는 경우는 거의 없음</li>
<li>게다가 질문도 많아야 10개 이하 </li>
<li><blockquote>
<p><strong>단순 배열로 관리</strong>했으면 더 깔끔했을 것입니다... </p>
</blockquote>
</li>
</ul>
<h4 id="3-검증validation-중복">3. 검증(Validation) 중복</h4>
<ul>
<li>Zod를 사용해 Validation을 관리하고 있음에도,</li>
<li><em>별도로 유효성 검사를 추가*</em>하여 코드가 불필요하게 길어짐</li>
</ul>
<h4 id="4-컴포넌트-응집도-최악">4. 컴포넌트 응집도 최악</h4>
<ul>
<li>ManageForm에서 모든 로직을 떠맡음</li>
<li>심지어 &quot;수정 버튼의 상태&quot;까지도 관리함</li>
<li><em>단일 책임 원칙(SRP)을 완전히 위반*</em>.. 하였어요</li>
</ul>
<h4 id="5-재사용-불가능한-거대한-컴포넌트">5. 재사용 불가능한 거대한 컴포넌트</h4>
<ul>
<li><strong>Sections, Question, FormEditButtons</strong> 등 모든 하위 컴포넌트가 과도한 props를 받음</li>
<li><strong>isEditing, isClosed, isPastStartDate, handleCreateForm</strong> 등 props를 남발하여 유지보수가 힘듦</li>
</ul>
<hr />
<h3 id="왜-이렇게-됐을까-반성">왜 이렇게 됐을까? (반성)...</h3>
<h4 id="1-수정에-대한-설계를-하지-않음">1. &quot;수정&quot;에 대한 설계를 하지 않음</h4>
<ul>
<li>처음에는 지원서 생성만 고려하여 개발</li>
<li>이후 &quot;수정&quot; 기능 추가 시 무리한 상태 추가 → 코드가 엉망이 됨</li>
</ul>
<h4 id="2-초기에는-모집-기간이-지난-폼지도-수정-가능했음">2. 초기에는 모집 기간이 지난 폼지도 수정 가능했음</h4>
<ul>
<li>이후 &quot;모집 기간이 지난 폼지는 수정 불가&quot; 정책이 추가됨</li>
<li>이 과정에서 상태가 더 복잡해짐</li>
</ul>
<h4 id="3-수정-기능-추가-시-기존-코드에-덧붙이는-방식으로-작성">3. 수정 기능 추가 시 기존 코드에 덧붙이는 방식으로 작성</h4>
<ul>
<li>&quot;기존 코드 유지&quot;하면서 기능 추가</li>
<li>결과적으로 &quot;일관성 없는 상태 관리&quot; + &quot;가독성 최악&quot;</li>
</ul>
<h4 id="4-데이터-구조-설계-실수">4. 데이터 구조 설계 실수</h4>
<ul>
<li>폼지의 필드를 섹션별 객체로 나눠 관리함</li>
<li>하지만 필드가 많아야 10개 이하 → 단순 배열로 관리하는 게 나았음</li>
</ul>
<p>일단 배포가 되었지만.... <strong>코드의 심각성</strong>🤮을 느끼고 
리팩토링에 들어가게 됩니다..... </p>
<hr />
<h3 id="리팩토링">리팩토링</h3>
<h4 id="1-상태-관리-일원화-formstate">1. 상태 관리 일원화 (formState)</h4>
<ul>
<li>기존에는 개별 상태 (title, description 등)를 각각 관리했지만, formState 객체로 통합하여 상태 변경을 일관되게 유지할 수 있음.</li>
<li>useState를 통해 모든 데이터를 한 곳에서 관리하면서도, <strong>특정 필드만 업데이트하는 방식</strong>으로 개선함.</li>
</ul>
<pre><code>const [formState, setFormState] = useState&lt;FormState&gt;({
    title: formData?.title ?? '',
    description: formData?.description ?? '',
    hasInterview: formData?.hasInterview ?? false,
    sections: formData?.sections ?? ['공통'],
    startDate: formData?.startDate ?? null,
    endDate: formData?.endDate ?? null,
    formFields: formData?.formFields ?? [],
  });</code></pre><h4 id="2-mode와-isdisabled-iseditableregardlessofperiod-활용">2. mode와 isDisabled, isEditableRegardlessOfPeriod 활용</h4>
<ul>
<li>mode가 <strong>수정(edit)과 뷰(view)</strong>를 명확하게 구분하도록 해 모드 전환이 직관적으로 변경했어요 </li>
</ul>
<p><strong>isDisabled</strong>와 <strong>isEditableRegardlessOfPeriod</strong>를 활용하여 사용자의 입력 가능 여부를 상태로 관리하므로,
폼 입력 필드나 버튼을 제어할 때 중복 로직 없이 깔끔하게 처리할 수 있음.</p>
<pre><code>
const isPastStartDate = formData?.startDate
    ? new Date(formData.startDate) &lt; new Date()
    : false;

  const [mode, setMode] = useState&lt;'view' | 'edit'&gt;(
    formData == undefined ? 'edit' : 'view',
  );

  const isDisabled = mode === 'view' || isPastStartDate;

  const isEditableRegardlessOfPeriod = mode === 'view';</code></pre><h4 id="3-formeditbuttons-컴포넌트로-책임-분리">3. FormEditButtons 컴포넌트로 책임 분리</h4>
<ul>
<li><p>기존에는 <strong>ManageForm에</strong>서 폼 저장 및 수정 로직을 직접 관리했지만, 이를 <strong>FormEditButtons</strong>로 이동하여 관심사 분리 (Separation of Concerns, SoC) 를 적용.</p>
</li>
<li><p><strong>FormEditButtons</strong> 내부에서 <strong>handleCreateForm, handleUpdateForm, onClickEditButton</strong> 등 관련 로직을 모두 처리하도록 구조화.</p>
</li>
</ul>
<p>-&gt; 이렇게 분리하면 ManageForm이 폼 데이터를 렌더링하는 역할에 집중할 수 있어 코드가 더 모듈화됨.</p>
<pre><code>import React from 'react';
import { useCookies } from 'react-cookie';
import { useNewForm } from '@/hooks/api/apply/useNewForm';
import { useUpdateForm } from '@/hooks/api/apply/useUpdateForm';
import { useUpdateFormDeadline } from '@/hooks/api/apply/useUpdateFormDeadline';
import { FormState } from '@/types/form';

type ModeType = 'view' | 'edit';

type Props = {
  formData: FormState | undefined;
  mode: ModeType;
  onReset: () =&gt; void;
  setMode: React.Dispatch&lt;React.SetStateAction&lt;ModeType&gt;&gt;;
  formState: FormState;
  id: number | undefined;
  isPastStartDate: boolean;
};

export default function FormEditButtons({
  isPastStartDate,
  formData,
  mode,
  onReset,
  setMode,
  formState,
  id,
}: Props) {
  const [{ token }] = useCookies(['token']);
  const newFormMutation = useNewForm(token);
  const updateFormMutation = useUpdateForm(setMode);
  const updateFormDeadlineMutation = useUpdateFormDeadline();

  const onClickEditButton = () =&gt; {
    setMode('edit');
  };
  const handleCreateForm = () =&gt; {
    newFormMutation.mutate({
      ...formState,
      startDate: formState.startDate || '',
      endDate: formState.endDate || '',
    });
  };
  const onClickCancelButton = () =&gt; {
    setMode('view');
    onReset();
  };
  const handleUpdateForm = () =&gt; {
    if (id === undefined) {
      return;
    }
    if (isPastStartDate) {
      updateFormDeadlineMutation.mutate({
        token,
        formId: id,
        endDate: formState.endDate || '',
      });
    } else {
      updateFormMutation.mutate({
        token,
        formId: id,
        formData: {
          ...formState,
          startDate: formState.startDate || '',
          endDate: formState.endDate || '',
        },
      });
    }
  };

  return (
    &lt;div className=&quot;mt-7 flex items-center justify-between gap-2 text-lg&quot;&gt;
      {!formData ? (
        &lt;button
          className=&quot;rounded-xl bg-blue-500 px-4 py-2 font-semibold text-white hover:bg-blue-600&quot;
          onClick={handleCreateForm}
        &gt;
          저장하기
        &lt;/button&gt;
      ) : (
        &lt;&gt;
          {mode == 'view' ? (
            &lt;button
              onClick={onClickEditButton}
              className=&quot;cursor-pointer rounded-xl bg-blue-100 px-4 py-2 font-semibold text-blue-500 hover:bg-blue-200&quot;
            &gt;
              수정하기
            &lt;/button&gt;
          ) : (
            &lt;div className=&quot;flex flex-row gap-2&quot;&gt;
              &lt;button
                onClick={onClickCancelButton}
                className=&quot;rounded-xl bg-gray-100 px-3 py-2 font-semibold text-gray-500 hover:bg-gray-200&quot;
              &gt;
                취소
              &lt;/button&gt;
              &lt;button
                onClick={handleUpdateForm}
                className=&quot;rounded-xl bg-blue-500 px-4 py-2 font-semibold text-white hover:bg-blue-400&quot;
              &gt;
                저장하기
              &lt;/button&gt;
            &lt;/div&gt;
          )}
        &lt;/&gt;
      )}
    &lt;/div&gt;
  );
}</code></pre><p>_아직도 수정할 부분이 많이 남았지만 일차적으로 <strong>구조적인 수정</strong>이 끝났습니다 
계속해서 리팩토링이 필요해보입니다 _</p>
<p>더 개선하고 싶은 부분은..</p>
<ol>
<li><p><strong>상태관리 최적화</strong> -&gt; formState를 useState로 관리하지만, 여러 필드 업데이트를 처리하면서 setState를 연속 호출하는 경우가 많음. 폼지에서 가장 필요한 부분이 아닐까 싶습니다</p>
</li>
<li><p><strong>formFields 업데이트 최적화</strong>
현재 formFields는 배열이라 추가/삭제 시마다 setFormState를 새로 호출하는 방식인데,불변성 유지 코드를 더 간결하게 수정하고 싶습니다 </p>
</li>
<li><p>*<em>불필요한 리렌더링 줄이기 *</em> : 현재 불필요한 리렌더링이 많이 일어나고 있어서 줄이고자 합니다 </p>
</li>
</ol>
<hr />
<h3 id="느낀-점">느낀 점</h3>
<blockquote>
<p>이번 리팩토링을 진행하면서 단순히 기능을 구현하는 것보다 <strong>코드의 구조를 정리</strong>하고 <strong>유지보수성을 높이는 것</strong>이 얼마나 중요한지 다시금 깨닫게 되었습니다.
처음에는 하나씩 기능을 추가하는 방식으로 개발했지만, 점점 코드가 복잡해지면서 유지보수하기 어려워졌고, 새로운 요구사항이 생길 때마다 예상보다 더 많은 수정이 필요하게 되었습니다.
이 과정에서 <strong>개발 초기 설계의 중요성</strong>을 체감했고, <strong>코드가 확장될 때도 유연하게 대응</strong>할 수 있도록 구조를 잡는 것이 필수적이라는 점을 배웠습니다.</p>
</blockquote>
<p>특히,</p>
<ul>
<li><strong>상태를 어떻게 관리</strong>할 것인지</li>
<li><strong>컴포넌트의 역할</strong>을 어떻게 나눌 것인지</li>
<li>어떤 <strong>로직을 어디에서 담당</strong>하도록 할 것인지
이러한 고민을 깊이 해보면서 단순히 &quot;동작하는 코드&quot;가 아닌 <strong>잘 설계된 코드</strong>가 무엇인지 고민하는 계기가 되었습니다.</li>
</ul>
<p>리팩토링을 통해 관심사를 분리하고, 불필요한 상태 관리를 줄이고, 코드의 가독성을 높이며, 유지보수가 쉬운 구조로 개선할 수 있었지만, 여전히 더 다듬을 부분이 남아 있다고 생각합니다.</p>
<p><strong>&quot;기능 구현&quot;을 넘어서 &quot;설계와 유지보수&quot;까지 고려하는 개발자가 되어야겠다!</strong>는 목표를 다시 한번 되새기게 된 리팩토링이었습니다. .. 다음 글은 최적화로 찾아뵙겠습니당 🙇‍♀️</p>